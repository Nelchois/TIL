This code is smilar to Sample1, but this one doesn't crawling url. 
```
# 라이브러리 import
import os
import pandas as pd
import numpy as np
import math 

# 셀레늄
from selenium.webdriver.common.alert import Alert
from selenium import webdriver  # 라이브러리(모듈) 가져오라
# pip install chromedriver_autoinstaller
from selenium.webdriver.common.keys import Keys
from selenium.webdriver import ActionChains as AC

# tqdm : for문 진행상황 체크
from tqdm import tqdm, tqdm_notebook
from tqdm.notebook import tqdm

# 정규표현식(regular expression) : 문자(알파벳,한글), 숫자, 특수기호 정제 및 추출
import re
from time import sleep
import time

# 워닝 무시
import warnings
warnings.filterwarnings('ignore')

# 영화배우 이름 검색
name = '배우 이정재'

# 크롬 옵션
options = webdriver.ChromeOptions()  

# 크롬 윈도우 사이즈 조절
options.add_argument("--window-size=800,1200") # window-size -> 기본 : 1920,1080

import chromedriver_autoinstaller
chrome_path = chromedriver_autoinstaller.install()
driver = webdriver.Chrome(chrome_path, options=options)
time.sleep(3)
driver.get("https://www.naver.com")

# 네이버 검색어 입력 후 검색
element = driver.find_element_by_name("query")
element.clear()  # 혹시 검색창에 존재하는 텍스트 제거

element.send_keys(name)  # 검색창에 검색어 전달
element.submit()  # 검색 클릭
time.sleep(1)

# 프로필 클릭
driver.find_element_by_link_text("프로필").click()
time.sleep(1)

# 수상내역 더보기 클릭
try:
    driver.find_element_by_css_selector(".cm_content_area._cm_content_area_prize .area_button_arrow").click()
    time.sleep(1)
except:
    pass

# 수상내역 갯수 세기
prize_list = []

for i in range(0, 100):  # 수상내역 페이지 최대 100페이지까지 수집할 수 있게 설정
    try:
        # 수상내역 크롤링
        prize = driver.find_element_by_css_selector(".cm_content_area._cm_content_area_prize").text
        # 년도만 세기
        p = re.compile('[0-9]{4,4}')   # 정규표현식으로 "0-9숫자 4자리" 수집
        m = p.findall(prize)           # prize 중에서 모두 찾아라(findall 함수)
        prize_list = prize_list + m    # 페이지 별 수상내역 리스트 전부 합치기

        # 수상내역 다음페이지 클릭
        if driver.find_element_by_css_selector('.pg_next.on'):
            next = driver.find_element_by_css_selector('.pg_next.on').click()
            time.sleep(2)
    except:  
        break
    
print('수상 수: ', len(prize_list))
prize_list
```
