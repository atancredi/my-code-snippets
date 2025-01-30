```python
from selenium import webdriver
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.remote.webdriver import WebDriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC

# Preparation

options = Options()

options.binary_location = "/usr/lib/firefox/firefox"

if not DEBUG:
	options.add_argument("--headless")

browser = webdriver.Firefox(options=options)

browser.get("https://www.instagram.com/accounts/login/")
```
