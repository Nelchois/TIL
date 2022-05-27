This code is Crawling code from website like social community. 
I apply CrawlingSample.md's code that using URL, however it pretty changed because structure of website is totally different.
```
import sys # 시스템
import os  # 시스템

import pandas as pd  # 판다스 : 데이터분석 라이브러리
import numpy as np   # 넘파이 : 숫자, 행렬 데이터 라이브러리

from bs4 import BeautifulSoup    # html 데이터를 전처리
from selenium import webdriver   # 웹 브라우저 자동화
from selenium.webdriver.common.keys import Keys
import chromedriver_autoinstaller

import time    # 서버와 통신할 때 중간중간 시간 지연. 보통은 1초
from tqdm import tqdm_notebook   # for문 돌릴 때 진행상황을 %게이지로 알려준다.

# 워닝 무시
import warnings
warnings.filterwarnings('ignore')
query_txt = '마듀갤'

import chromedriver_autoinstaller
chrome_path = chromedriver_autoinstaller.install()
driver = webdriver.Chrome(chrome_path)

# 사이트 주소는 네이버
driver.get('http://www.google.com')
time.sleep(2)  # 2초간 정지
element = driver.find_element_by_css_selector("body > div.L3eUgb > div.o3j99.ikrT4e.om7nvf > form > div:nth-child(1) > div.A8SBwf > div.RNNXgb > div > div.a4bIc > input")
element.send_keys(query_txt)  # query_txt는 위에서 입력한 키워드
element.submit()
time.sleep(1)
driver.find_element_by_css_selector('#rso > div:nth-child(1) > div > div:nth-child(1) > div > div > div.NJo7tc.Z26q7c.jGGQ5e > div > a > h3').click( )
time.sleep(1)
driver.find_element_by_xpath('//*[@id="container"]/section[1]/article[2]/div[1]/div[1]/button[2]').click()
time.sleep(1)
item_li = []
item_li = driver.find_elements_by_css_selector('.inner > ul > li')
for i in range(0, len(item_li)):
    print(item_li[i].text)

print(item_li[6].text)
item_li[6].click()

def scroll_down(driver):
    driver.execute_script("window.scrollTo(0, 99999999)")
    time.sleep(1)
n = 5 
i = 0
while i < n: 
    scroll_down(driver)
    i = i+1

url_list = []
title_list = []


for i in range(3, 53):
    article_raw = driver.find_elements_by_css_selector(f"#container > section.left_content > article:nth-child(3) > div.gall_listwrap.list > table > tbody > tr:nth-child({i}) > td.gall_tit.ub-word > a:nth-child(1)")
    url = article_raw[0].get_attribute('href')
    url_list.append(url)

url_list

target = []

for i in tqdm_notebook(range(0, 50)):
    url = url_list[i]
    chrome_path = chromedriver_autoinstaller.install()
    driver = webdriver.Chrome(chrome_path)
    
    driver.get(url)
    time.sleep(1)
    if i == 1:
        result_df = pd.DataFrame(target_info)
    elif i > 1:
        addiction_df = pd.DataFrame(target_info)
        result_df = pd.concat([result_df, addiction_df])
    
    try : 
        target_info = []  

        overlays = ".title_subject"                                
        tit = driver.find_element_by_css_selector(overlays)        
        title = [tit.text]
    
                                
        nick = driver.find_element_by_css_selector('#container > section > article:nth-child(3) > div.view_content_wrap > header > div > div > div.fl > span.nickname.in > em')        
        nickname = [nick.text]

                               
        date = driver.find_element_by_css_selector('#container > section > article:nth-child(3) > div.view_content_wrap > header > div > div > div.fl > span.gall_date')         
        datetime = [date.text]

        overlays = ".write_div"                                 
        contents = driver.find_elements_by_css_selector(overlays)   
        
        content_list = []
        for content in contents:
            content_list.append(content.text)
    
        target_info = zip(title, nickname, datetime, content_list)
           
        
        target.append(target_info)
        time.sleep(1)       
        
        print(i, title, nickname)
        driver.close() 
    
    except:
        driver.close()
        time.sleep(1)
        continue
    
    
result_df.to_csv("gall_content.csv", encoding='utf-8-sig')
print('수집한 글 갯수: ', len(target))

result_df.columns = ['title', 'nickname', 'datetime', 'contents']

result_df['contents'] = result_df['contents'].str.replace('\n', '')

result_df
```
The last three line is code about make DataFrame with pandas and save it to CSV file.