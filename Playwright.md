![Playwright](https://cdn.svglogos.dev/logos/playwright.svg)

---

# Installation
```bash
pip install playwright
playwright install
```

---

# Boilerplate

## Synchronous
```python
from playwright.sync_api import sync_playwright
from time import time
from dotenv import load_dotenv
import os

load_dotenv()

class PlaywrightScraper:
	def __init__(self, headless=False):
		self.headless = headless
		self.playwright = None
		self.browser = None
		self.page = None
		self.start_time = None
		self.timelapse = None
		self.data = []
	
	def __enter__(self):
		self.start_time = time()
		print('Starting browser...')
		self.playwright = sync_playwright().start()
		self.browser = self.playwright.chromium.launch(headless=self.headless)
		self.page = self.browser.new_page()
		return self
	
	def fetch_page(self, url):
		self.page.goto(url)
	
	def login(self):
		username = os.getenv('USERNAME')
		password = os.getenv('PASSWORD')
		
		self.page.locator('#student_uid').fill(username)
		self.page.locator('#password').fill(password)
		self.page.locator('#submit').click()
	
	def run(self, url):
		self.fetch_page(url)
		self.timelapse = round(time() - self.start_time, 2)
		return (self.timelapse, self.data)
	
	def __exit__(self, *args):
		print('Closing browser...')
		self.browser.close()
		self.playwright.stop()
		print(f'Time taken: {self.timelapse}s')

def main():
    with PlaywrightScraper() as scraper:
		url = 'https://izone.sunway.edu.my/timetable'
		scraper.run(url)

if __name__ == '__main__':
	main()
```

## Asynchronous
- Add `await`
- Run with `asyncio.run()`
```python
from playwright.async_api import async_playwright
from time import time
from dotenv import load_dotenv
import os
import asyncio

load_dotenv()

class PlaywrightScraperAsync:
    def __init__(self, headless=False):
        self.headless = headless
        self.playwright = None
        self.browser = None
        self.page = None
        self.start_time = None
        self.timelapse = None
        self.data = []

    async def __aenter__(self):
        self.start_time = time()
        print('Starting browser...')
        self.playwright = await async_playwright().start()
        self.browser = await self.playwright.chromium.launch(headless=self.headless)
        self.page = await self.browser.new_page()
        return self

    async def fetch_page(self, url):
        await self.page.goto(url)

    async def login(self):
        username = os.getenv('USERNAME')
        password = os.getenv('PASSWORD')

        await self.page.locator('#student_uid').fill(username)
        await self.page.locator('#password').fill(password)
        await self.page.locator('#submit').click()

    async def run(self, url):
        await self.fetch_page(url)
        self.timelapse = round(time() - self.start_time, 2)
        return (self.timelapse, self.data)

    async def __aexit__(self, *args):
        print('Closing browser...')
        await self.browser.close()
        await self.playwright.stop()
        print(f'Time taken: {self.timelapse}s')

async def main():
    async with PlaywrightScraperAsync() as scraper:
        url = 'https://izone.sunway.edu.my/timetable'
        await scraper.run(url)

if __name__ == '__main__':
    asyncio.run(main())
```

---

# Target Elements

## Locators
```python
# CSS selectors
page.locator('#login')
page.locator('.btn-primary')
page.locator('input[name="username"]')
page.locator('div > span')
page.locator('ul li:nth-child(2)')

# Chained locators
page.locator('form').locator('input[name="email"]')

# Has
page.locator('div', has=page.locator('input'))

# Index
page.locator('li').first
page.locator('li').last
page.locator('li').nth(2)
page.locator('li:first-child')
page.locator('li:last-child')
page.locator('li:nth-child(2)')
```

## By Text
```python
page.get_by_text('Login')
page.get_by_text('Login', exact=True)
# <button>Login</button>
```

## By Role (Aria)
```python
page.get_by_role('button', name='Submit')
# <button>Submit</button>
# <button aria-label="Submit"></button>
```

## By XPath
```python
# Exact vs partial match
page.locator('//input[@id="password"]')
page.locator('//input[@type="text" and @name="username"]')
page.locator('//div[contains(@class, "active")]')
page.locator('//input[starts-with(@id, "user")]')

# Match text
page.locator('//button[text()="Login"]')
page.locator('//button[contains(text(), "Log")]')

# Child
page.locator('//form//input')
page.locator('//form/input')  # Direct child

# Indexing
page.locator('(//li)[1]')
page.locator('(//li)[last()]')
```

## By XPath (Siblings)
```html
<div>
	<label>Username</label>
	<input id="username"/>
	<label>Password</label>
	<input id="password"/>
</div>
```
```python
# Preceding sibling (All labels before the element)
page.locator('//input[@id="password"]/preceding-sibling::label')
# <label>Username</label>
# <label>Password</label>

# Following sibling (All inputs after the element)
page.locator('//label[text()="Username"]/following-sibling::input')
# <input id="username"/>
# <input id="password"/>

# Just the closest one
page.locator('//input[@id="password"]/preceding-sibling::label[1]')
```

## By Xpath (Ancestors)
```html
<form id="login-form">
	<div>
		<label>Username</label>
		<input id="username"/>
	</div>
</form>
```
```python
page.locator('//input[@id="username"]/ancestor::form')
page.locator('//form[@id="login-form"]/ancestor-or-self::form')
```

## By Others
```python
page.get_by_label('Username')
page.get_by_placeholder('Enter username')
page.get_by_alt_text('Company logo')
page.get_by_title('Issues count')

# Combine
button = page.get_by_role('button').and_(page.get_by_title('Sub'))
```

## Wait for Elements
```python
page.get_by_role('button').wait_for()
```

---

# Page Navigation
```python
page.goto(
    'https://example.com',
    wait_until='load',  # load, domcontentloaded, networkidle
    timeout=30000  # 30s
)

page.reload(wait_until='networkidle')
page.go_back()
page.go_forward()
page.url
page.wait_for_url('**/dashboard', timeout=10000)

# New page
new_page = browser.new_page()
new_page.goto('https://example.com/new')
```

## Wait for Popup
```python
with page.expect_popup() as popup_info:
    page.click('a#open-new-tab')
new_page = popup_info.value
```

## iFrame
```python
frame = page.frame(name='login-frame')
frame.locator('#submit')
```

---

# Context
- Isolated session
```python
context1 = browser.new_context()
context2 = browser.new_context()

page1 = context1.new_page()
page2 = context2.new_page()

# Instead of just
page = browser.new_page()
```

---

# Interactions
```python
# Clicks
el.click()
el.dblclick()
el.click(button='right')  # Right click
el.click(click_count=2, delay=100)

# Keys
el.fill('myuser')
el.press('Enter')  # Tab, ArrowDown, Escape
el.type('myuser', delay=100)

# Select
el.select_option('MY')  # <option value="MY">
el.select_option(['MY', 'SG'])

# Checkbox, Radio buttons
el.check()
el.uncheck()

# Drag and drop
source = page.locator('#source')
target = page.locator('#target')
source.drag_to(target)

# Drag sliders
slider = page.locator('#volume')
slider.drag_to(slider, target_position={'x': 50, 'y': 0})

# Focus, remove focus
el.focus()
el.blur()

# Hover on element
el.hover()

# Scroll
el.scroll_into_view_if_needed()

# Screenshot
el.screenshot(path='chart.png')

# Sleep
page.wait_for_timeout(1000)  # sleep 1s

# Element status
print(el.is_visible())
print(el.is_enabled())
print(el.is_disabled())
print(el.is_checked())

# Element properties
print(el.text_content())
print(el.inner_text())
print(el.get_attribute('value'))

# Dimensions
box = el.bounding_box()
print(box['x'], box['y'], box['width'], box['height'])
```

## Page Level Actions
```python
# Keys
page.keyboard.press('Tab')
page.keyboard.type('Hello')

# Mouse
page.mouse.click(100, 200)
page.mouse.move(50, 50)
page.mouse.down()
page.mouse.up()
page.mouse.wheel(0, 100)

# Screenshot
page.screenshot(path='fullpage.png', full_page=True)
page.pdf(path='output.pdf')
```

## Screen Record
```python
context = browser.new_context(
	record_video_dir='videos/',
	record_video_size={'width': 1280, 'height': 720}
)

page = context.new_page()
page.goto('https://example.com')
page.close()

context.close()
```

---

# Assertions
```python
from playwright.sync_api import expect

expect(el).to_be_visible()
expect(el).to_be_enabled()
expect(el).to_be_disabled()
expect(el).to_be_hidden()

expect(el).to_have_text('Hello World')
expect(el).to_contain_text('Hello')
expect(el).to_have_value('myuser')
expect(el).to_have_count(3)
expect(el).to_have_attribute('type', 'submit')
expect(el).to_have_class('btn-primary')
expect(el).to_have_css('color', 'rgb(0, 0, 0)')

expect(el).to_be_checked()
expect(el).not_to_be_checked()
expect(el).to_be_selected()

expect(el).to_be_focused()
expect(el).not_to_be_focused()
```

## Page Level Assertions
```python
expect(page).to_have_url('https://example.com/dashboard')
expect(page).to_have_title('Dashboard')
```

---

# Download Files
```python
with page.expect_download() as download_info:
	page.locator('#download-btn').click()

download = download_info.value
download.save_as('downloads/' + download.suggested_filename)
```

---

# Run JavaScript
```python
page.evaluate('document.querySelector("#login").click()')
page.evaluate('() => console.log("Hello from JS")')

# Can pass in element
value = el.evaluate('(element) => element.value')
```

## Inject External JavaScript File
```python
# Add js file
page.add_script_tag(path='script.js')
result = page.evaluate('sayHi()')

# Add bootstrap
page.add_script_tag(url='https://code.jquery.com/jquery-3.6.0.min.js')

# js code in python
js_code = '''
	function multiply(a, b) {
	    return a * b;
	}
'''
page.add_script_tag(content=js_code)
```

