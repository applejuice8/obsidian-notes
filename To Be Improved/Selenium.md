![Selenium](https://cdn.svglogos.dev/logos/selenium.svg)

---

# Boilerplate
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC

from time import time
from dotenv import load_dotenv
import os

load_dotenv()

class SeleniumScraper:
    def __init__(self, headless=False):
        self.headless = headless
        self.driver = None
        self.wait = None
        self.start_time = None
        self.timelapse = None
        self.data = []

    def __enter__(self):
        self.start_time = time()
        print('Starting browser...')

        options = Options()
        if self.headless:
            options.add_argument('--headless=new')

        self.driver = webdriver.Chrome(
            service=ChromeService(ChromeDriverManager().install()),
            options=options
        )
        self.wait = WebDriverWait(self.driver, 10)
        return self

    def fetch_page(self, url):
        self.driver.get(url)

    def login(self):
        username = os.getenv('USERNAME')
        password = os.getenv('PASSWORD')

        self.wait.until(
	        EC.presence_of_element_located((By.ID, 'student_uid'))
		)

        self.driver.find_element(By.ID, 'student_uid').send_keys(username)
        self.driver.find_element(By.ID, 'password').send_keys(password)
        self.driver.find_element(By.ID, 'submit').click()

    def run(self, url):
        self.fetch_page(url)
        self.timelapse = round(time() - self.start_time, 2)
        return (self.timelapse, self.data)

    def __exit__(self, *args):
        print('Closing browser...')
        self.driver.quit()
        print(f'Time taken: {self.timelapse}s')

def main():
    with SeleniumScraper() as scraper:
        url = 'https://izone.sunway.edu.my/timetable'
        scraper.run(url)

if __name__ == '__main__':
    main()
```

---

# Options
```python
from selenium.webdriver.chrome.options import Options

options = Options()

options.add_argument("--headless=new")
options.add_argument("--disable-gpu")
options.add_argument("--window-size=1920,1080")
options.add_argument("--incognito")
options.add_argument("--guest")

# Wait for page to load
options.page_load_strategy = "normal"  # Default
options.page_load_strategy = "eager"  # Stops after DOMContentLoaded
options.page_load_strategy = "none"  # Returns immediately
```

---

# Singular vs Plural
```python
button = driver.find_element(By.TAG_NAME, 'button')
buttons = driver.find_elements(By.TAG_NAME, 'button')
```
