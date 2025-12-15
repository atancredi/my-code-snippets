```python
from urllib.parse import urlparse, parse_qs
from typing import Optional

class UTMParameters:
    utm_source: str
    utm_medium: Optional[str] = None
    utm_campaign: Optional[str] = None
    utm_term: Optional[str] = None
    utm_content: Optional[str] = None

    def to_query_string(self) -> str:
        s = f"utm_source={self.utm_source}"
        if self.utm_medium:
            s += f"&utm_medium={self.utm_medium}"
        if self.utm_campaign:
            s += f"&utm_campaign={self.utm_campaign}"
        if self.utm_term:
            s += f"&utm_term={self.utm_term}"
        if self.utm_content:
            s += f"&utm_content={self.utm_content}"
        return s

    def parse_query_string(self, _query):
        r = parse_qs(urlparse(_query).query)
        self.utm_source = r["utm_source"][0]
        self.utm_medium = r["utm_medium"][0]
        self.utm_campaign = r["utm_campaign"][0]
        self.utm_term = r["utm_term"][0]
        self.utm_content = r["utm_content"][0]
```