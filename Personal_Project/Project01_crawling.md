This code is crawling part.

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup
import time
from tqdm import tqdm_notebook
import pandas as pd
import numpy as np

browser=webdriver.Chrome()
browser.implicitly_wait(10)
browser.get("http://www.google.com")
browser.implicitly_wait(10)
l = browser.find_element_by_xpath('/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input')
l.send_keys("스팀 이터널리턴\n")
browser.implicitly_wait(10)
l = browser.find_element_by_css_selector('#rso > div:nth-child(1) > div > div.NJo7tc.Z26q7c.jGGQ5e > div > a > div > cite')
l.click()
browser.implicitly_wait(10)
browser.execute_script("window.scrollTo(0, 10000);")
l = browser.find_element_by_xpath('//*[@id="ViewAllReviewssummary"]/a')
l.click()
browser.implicitly_wait(10)
l = browser.find_element_by_xpath('//*[@id="filterselect_activeday"]')
l.click()
browser.implicitly_wait(10)
l = browser.find_element_by_xpath('//*[@id="filterselect_option_4"]')
l.click()
browser.implicitly_wait(10)
l = browser.find_element_by_xpath('//*[@id="filterlanguage_activeday"]')
l.click()
browser.implicitly_wait(10)
l = browser.find_element_by_xpath('//*[@id="filterlanguage_option_5"]')
l.click()

def steam(browser, k):
    title_list = []
    playtime_list = []
    card_text_list = []
    html = browser.page_source
    soup = BeautifulSoup(html,'html.parser')
    total = soup.select('.apphub_UserReviewCardContent')
    for card_box in total:
        title = card_box.select_one('.title')
        title_list.append(title.text)
        playtime = card_box.select_one('.hours')
        playtime_list.append(playtime.text)
        card_text = card_box.select_one('.apphub_CardTextContent')
        card_text_list.append(card_text.text)
        for i in range(k):
            instant_review_list = zip(title_list, playtime_list, card_text_list)
            if i == 0:
                df = pd.DataFrame(instant_review_list, columns = ['recommend', 'playtime', 'review'])
            elif i != 0:
                add_df = pd.DataFrame(instant_review_list, columns = ['recommend', 'playtime', 'review'])
                review_df = pd.concat([df, add_df])
        data = review_df
    return data 

def scroll_down(browser):
    browser.execute_script("window.scrollTo(0, 99999999)")
    time.sleep(1)

n = 50 
i = 0
while i < n: 
    i = i+1
    scroll_down(browser)
    data = steam(browser, n)
    time.sleep(1)

print(data)
```
Comment : There's nothing to explain in browser part. 
In function, I use Beautiful soup, because there are a lot of same class in this page, so it's difficult to access a specific line that using xpath or selector.

Next is scroll method. 
This page is total page that if user scroll down then the page renewal document automatically, so there isn't method for switch page.
I use beautiful soup to get all documents that browser is locked and make scroll down.