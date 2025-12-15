
### Fill text field
```python
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium import webdriver

locator=(By.ID,"id")
def fill_field(browser,locator,text,enter=False):
	element = browser.find_element(*locator)
	element.clear()
	element.send_keys(text)
	if enter:
		element.send_keys(Keys.ENTER)
```

### Wait and select element
```python
locator = (By.XPATH,"")
def sel(browser, locator, timeout = 10):
	WebDriverWait(browser, timeout).until(EC.visibility_of_element_located(locator))
	return browser.find_element(*locator)
```

### Wait for page URL change
```python
WebDriverWait(browser, 10).until(lambda driver: driver.current_url != browser.current_url)
```