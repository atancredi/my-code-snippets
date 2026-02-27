# Estimate size of a request payload

This is useful in determining whether to split a payload over multiple requests.


```python
def estimate_request_size(payload):
    json_bytes = json.dumps(payload, separators=(",", ":"), ensure_ascii=False).encode(
        "utf-8"
    )
    payload_size = len(json_bytes)

    print(f"JSON payload size: {payload_size} bytes, {payload_size*10e-3:.2f}KB, {payload_size*10e-6:.2f}MB")
```
