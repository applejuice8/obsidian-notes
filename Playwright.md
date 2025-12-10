
---

# Template
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

  

if __name__ == '__main__':

with PlaywrightScraper() as scraper:

url = 'https://izone.sunway.edu.my/timetable'

scraper.run(url)
```