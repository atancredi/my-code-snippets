# Execute JS code

```python
def execute_script(browser,code,path=None):
	if path != None:
		with open(path) as f:
			_s = f.read()
			print("script loaded")
	else:
		_s = code
	retval = browser.execute_async_script(_s)
	print("Done executing script")
	return retval
```