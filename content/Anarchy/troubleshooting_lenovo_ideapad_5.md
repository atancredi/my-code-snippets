
# troubleshooting power delivery on ideapad 5



- export battery report html in `C:/Users/<user>` with
```shell
powercfg /batteryreport # powershell
```

- export events from Windows Event Viewer with this filter query and export `.evtx` file
```xml
<QueryList>
  <Query Id="0" Path="System">
    <Select Path="System">*[System[Provider[@Name='ACPI' or @Name='Microsoft-Windows-Kernel-Power' or @Name='Microsoft-Windows-Kernel-PowerTrigger'] and TimeCreated[timediff(@SystemTime) &lt;= 604800000]]]</Select>
  </Query>
</QueryList>
```

- use the python script to combine the results of the two files to create a timeline near the flapping/controller panic events



```python
# pip install evtx pandas beautifulsoup4 lxml

import pandas as pd
from evtx import PyEvtxParser
import xml.etree.ElementTree as ET
import warnings

# Suppress pandas warnings
warnings.filterwarnings('ignore', category=FutureWarning)

def parse_evtx(evtx_path):
    print("Parsing EVTX file...")
    parser = PyEvtxParser(evtx_path)
    records = []
    ns = {'ns': 'http://schemas.microsoft.com/win/2004/08/events/event'}

    for record in parser.records():
        try:
            root = ET.fromstring(record['data'])
            event_id_elem = root.find('.//ns:EventID', ns)
            event_id = event_id_elem.text if event_id_elem is not None else None
            
            time_elem = root.find('.//ns:TimeCreated', ns)
            system_time = time_elem.attrib.get('SystemTime') if time_elem is not None else None
            
            records.append({
                'Time': pd.to_datetime(system_time),
                'Source': 'EVTX',
                'EventID': event_id,
                'RecordID': record['event_record_id'],
                'State': None,
                'Capacity_Pct': None
            })
        except Exception:
            continue
            
    df_evtx = pd.DataFrame(records)
    # Convert UTC to local time (Change 'Europe/Rome' if your system is in a different timezone)
    df_evtx['Time'] = df_evtx['Time'].dt.tz_convert('Europe/Rome').dt.tz_localize(None)
    return df_evtx

def parse_battery_report(html_path):
    print("Parsing Battery Report HTML...")
    tables = pd.read_html(html_path)
    
    target_table = None
    for tbl in tables:
        if 'START TIME' in tbl.columns and 'STATE' in tbl.columns:
            target_table = tbl
            break
            
    if target_table is None:
        raise ValueError("Could not find the 'Recent Usage' table.")
        
    df_batt = target_table.copy()
    df_batt['START TIME'] = pd.to_datetime(df_batt['START TIME'], errors='coerce')
    df_batt = df_batt.dropna(subset=['START TIME'])
    
    # Safely extract numeric battery capacity (removes % signs or mWh text)
    if 'CAPACITY REMAINING (%)' in df_batt.columns:
        df_batt['Capacity_Pct'] = df_batt['CAPACITY REMAINING (%)'].astype(str).str.extract(r'(\d+)').astype(float)
    else:
        df_batt['Capacity_Pct'] = None

    df_batt = df_batt.rename(columns={'START TIME': 'Time', 'STATE': 'State'})
    df_batt['Source'] = 'BatteryReport'
    df_batt['EventID'] = None
    df_batt['RecordID'] = None
    
    return df_batt[['Time', 'Source', 'EventID', 'RecordID', 'State', 'Capacity_Pct']]

def detect_flapping_windows(df_batt, window_minutes=10, max_state_changes=3, max_cap_jump_rate=5.0):
    print("Analyzing battery data for controller panic / flapping...")
    
    # Ensure it's sorted
    df_batt = df_batt.sort_values('Time').reset_index(drop=True)
    
    # 1. Detect rapid state changes (Count rows in a rolling time window)
    rolling_counts = df_batt.rolling(f'{window_minutes}min', on='Time').Source.count()
    
    # 2. Detect impossible capacity jumps (% change per minute)
    cap_diff = df_batt['Capacity_Pct'].diff().abs()
    time_diff = df_batt['Time'].diff().dt.total_seconds() / 60.0
    # Avoid division by zero
    rate_of_change = cap_diff / time_diff.replace(0, 0.1) 
    
    # Flag rows that meet the panic criteria
    is_flapping = (rolling_counts > max_state_changes) | (rate_of_change > max_cap_jump_rate)
    flapping_times = df_batt[is_flapping]['Time'].tolist()
    
    # Create time intervals (± 5 minutes around each flagged event)
    buffer = pd.Timedelta(minutes=5)
    intervals = [(t - buffer, t + buffer) for t in flapping_times]
    
    print(f"Detected {len(intervals)} signs of flapping/panic.")
    return intervals

def process_and_merge(evtx_path, html_path):
    df_evtx = parse_evtx(evtx_path)
    df_batt = parse_battery_report(html_path)
    
    # ==========================================
    # GOAL 1: DUMP ALL EVENTS INTO A SINGLE FILE
    # ==========================================
    print("\nMerging into a single master timeline...")
    full_timeline = pd.concat([df_evtx, df_batt], ignore_index=True)
    full_timeline = full_timeline.sort_values('Time').reset_index(drop=True)
    
    # Reorder columns for readability
    columns_order = ['Time', 'Source', 'EventID', 'State', 'Capacity_Pct', 'RecordID']
    full_timeline = full_timeline[columns_order]
    
    full_timeline_path = "full_system_timeline.csv"
    full_timeline.to_csv(full_timeline_path, index=False)
    print(f"Master timeline saved to {full_timeline_path}")

    # ==========================================
    # GOAL 2 & 3: ISOLATE FLAPPING & MERGE EVTX
    # ==========================================
    intervals = detect_flapping_windows(df_batt)
    
    if not intervals:
        print("No flapping or controller panics detected based on current thresholds.")
        return
        
    # Create a mask to filter the full timeline for only the "panic windows"
    mask = pd.Series(False, index=full_timeline.index)
    for start, end in intervals:
        mask = mask | ((full_timeline['Time'] >= start) & (full_timeline['Time'] <= end))
        
    panic_timeline = full_timeline[mask].copy()
    
    panic_path = "flapping_analysis_isolated.csv"
    panic_timeline.to_csv(panic_path, index=False)
    print(f"Isolated flapping events (merged with EVTX) saved to {panic_path}")
    
    print("\nPreview of Flapping Analysis Data:")
    print(panic_timeline.head(15).to_string(index=False))



# ==========================================
# EXECUTION
# ==========================================
if __name__ == "__main__":
    # Replace these paths with your actual file locations
    EVTX_FILE = "../battery-debug-events.evtx" 
    HTML_FILE = "../battery-report.html"
    
    process_and_merge(EVTX_FILE, HTML_FILE)
```
    

- feed the timeline to some LLM
