This code is sample of Crawling in website, so if you want to crawling some pages that use and refine this code.
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
# Step 1. 크롬 웹브라우저 실행

# pip install chromedriver_autoinstaller
import chromedriver_autoinstaller
chrome_path = chromedriver_autoinstaller.install()
driver = webdriver.Chrome(chrome_path)

# 사이트 주소는 네이버
driver.get('http://www.naver.com')
time.sleep(2)  # 2초간 정지

# Step 2. 네이버 검색창에 "검색어" 검색
element = driver.find_element_by_id("query")
element.send_keys(query_txt)  # query_txt는 위에서 입력한 키워드
element.submit()
time.sleep(1)

# 검색옵션 확인
item_li = driver.find_elements_by_css_selector('.option .txt')

for i in range(0, len(item_li)):
    print(item_li[i].text)

# 스크롤을 밑으로 내려주는 함수
def scroll_down(driver):
    driver.execute_script("window.scrollTo(0, 99999999)")
    time.sleep(1)

# n: 스크롤할 횟수 설정
n = 5  # 스크롤 1번 당 글 30개씩 화면에 보여짐
i = 0
while i < n: # 이 조건이 만족되는 동안 반복 실행
    scroll_down(driver) # 스크롤 다운
    i = i+1

# 블로그 글 url들 수집
url_list = []
title_list = []

# URL_raw 크롤링 시작
articles = ".api_txt_lines.total_tit"
article_raw = driver.find_elements_by_css_selector(articles)
article_raw[0]

# 크롤링한 url 정제 시작
for article in article_raw:
    url = article.get_attribute('href')
    url_list.append(url)
time.sleep(1)

# 제목 크롤링 시작
for article in article_raw:
    title = article.text
    title_list.append(title)

    print(title)

print("")
print('url갯수: ', len(url_list))
print('title갯수: ', len(title_list))

# 수집된 url_list, title_list로 판다스 데이터프레임 만들기
df = pd.DataFrame({'url':url_list, 'title':title_list})
df

# 저장하기
df.to_csv("blog_url.csv", encoding='utf-8-sig')
# df.to_csv("blog_url.csv")

import sys
import os
import pandas as pd
import numpy as np

dict = {}    # 전체 크롤링 데이터를 담을 그릇

# 수집할 글 갯수 정하기
number = 10

# 수집한 url 돌면서 데이터 수집
for i in tqdm_notebook(range(0, number)):
    # 글 띄우기
    url = url_load['url'][i]
    chrome_path = chromedriver_autoinstaller.install()
    driver = webdriver.Chrome(chrome_path)

    driver.get(url)   # 글 띄우기
    
    # 크롤링
    # 예외 처리
    try : 
        # iframe 접근
        driver.switch_to.frame('mainFrame')

        target_info = {}  # 개별 블로그 내용을 담을 딕셔너리 생성

        # 제목 크롤링
        overlays = ".se-module.se-module-text.se-title-text"                                
        tit = driver.find_element_by_css_selector(overlays)          # title
        title = tit.text

        # 글쓴이 크롤링
        overlays = ".nick"                                 
        nick = driver.find_element_by_css_selector(overlays)         # nickname
        nickname = nick.text

        # 날짜 크롤링
        overlays = ".se_publishDate.pcol2"                                 
        date = driver.find_element_by_css_selector(overlays)         # datetime
        datetime = date.text

        # 내용 크롤링
        overlays = ".se-component.se-text.se-l-default"                                 
        contents = driver.find_elements_by_css_selector(overlays)    # contents

        content_list = []
        for content in contents:
            content_list.append(content.text)
 
        content_str = ' '.join(content_list)                         # content_str

        # 글 하나는 target_info라는 딕셔너리에 담기게 되고,
        target_info['title'] = title
        target_info['nickname'] = nickname
        target_info['datetime'] = datetime
        target_info['content'] = content_str

        # 각각의 글은 dict라는 딕셔너리에 담기게 됩니다.
        dict[i] = target_info
        time.sleep(1)
        
        # 크롤링이 성공하면 글 제목을 출력하게 되고,
        print(i, title, nickname)

        # 글 하나 크롤링 후 크롬 창을 닫습니다.
        driver.close() 
    
    # 에러나면 현재 크롬창을 닫고 다음 글(i+1)로 이동합니다.
    except:
        # print("에러",i, title)
        driver.close()
        time.sleep(1)
        continue
    
    # 중간,중간에 파일로 저장하기
    if i==30 or i==50 or i==80:
        # 판다스 데이터프레임으로 만들기
        import pandas as pd
        result_df = pd.DataFrame.from_dict(dict, 'index')

        # 저장하기
        result_df.to_csv("blog_content.csv", encoding='utf-8-sig')
        time.sleep(3)

print('수집한 글 갯수: ', len(dict))
print(dict)

# 판다스로 만들기
import pandas as pd
result_df = pd.DataFrame.from_dict(dict, orient='index')
result_df
```