# Python `requests` Quick Reference

### Installation
```bash
pip install requests
```

### Basic Setup
```python
import requests
```

### GET Request
```python
response = requests.get('https://api.example.com/data')

print(response.status_code)
print(response.text)
print(response.json()) # If response is JSON
```

### GET with Query Parameters
```python
params = {'key1': 'value1', 'key2': 'value2'}
response = requests.get('https://api.example.com/search', params=params)
# URL becomes: https://api.example.com/search?key1=value1&key2=value2
```

### POST Request (Form Data)
```python
payload = {'username': 'test_user', 'password': 'test_password'}
response = requests.post('https://api.example.com/login', data=payload)
```

### POST Request (JSON Data)
```python
payload = {'username': 'test_user', 'role': 'admin'}
response = requests.post('https://api.example.com/users', json=payload)
```

### Custom Headers
```python
headers = {
    'Authorization': 'Bearer YOUR_TOKEN',
    'User-Agent': 'my-app/0.0.1',
    'Accept': 'application/json'
}
response = requests.get('https://api.example.com/protected', headers=headers)
```

### Basic Authentication
```python
from requests.auth import HTTPBasicAuth

response = requests.get('https://api.example.com/user', auth=HTTPBasicAuth('user', 'pass'))
# Or shorter:
response = requests.get('https://api.example.com/user', auth=('user', 'pass'))
```

### Timeouts
```python
# Timeout in seconds
try:
    response = requests.get('https://api.example.com/data', timeout=5.0)
except requests.exceptions.Timeout:
    print("The request timed out")
```

### Sessions (Persist parameters/cookies across requests)
```python
session = requests.Session()
session.headers.update({'Authorization': 'Bearer YOUR_TOKEN'})

# Both requests will use the headers set above
response1 = session.get('https://api.example.com/endpoint1')
response2 = session.get('https://api.example.com/endpoint2')
```

### Handling Cookies
```python
# Sending cookies
cookies = {'session_id': '123456789'}
response = requests.get('https://api.example.com', cookies=cookies)

# Reading cookies from response
print(response.cookies.get('session_id'))
```

### File Uploads
```python
files = {'file': open('document.pdf', 'rb')}
response = requests.post('https://api.example.com/upload', files=files)
```

### Downloading a File (Streaming)
```python
response = requests.get('https://api.example.com/largefile.zip', stream=True)

with open('largefile.zip', 'wb') as fd:
    for chunk in response.iter_content(chunk_size=128):
        fd.write(chunk)
```

### Error Handling
```python
response = requests.get('https://api.example.com/data')

try:
    # Raises requests.exceptions.HTTPError for 4XX or 5XX status codes
    response.raise_for_status() 
except requests.exceptions.HTTPError as err:
    print(f"HTTP error occurred: {err}")
except Exception as err:
    print(f"Other error occurred: {err}")
```