### Crawling subjects in page
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

### Extended Ver
This code is extended version of first code. It collect str information in pages and manage some 'str' number. Finally, it will make DataFrame and analyzed score.
```
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


# 필모그래피 클릭
driver.find_element_by_link_text("필모그래피").click()
time.sleep(1)

# 인기순 클릭
driver.find_element_by_link_text("인기순").click()
time.sleep(1)

film_num = int(driver.find_element_by_css_selector('.this_text_number').text)
print('필모 수: ', film_num)

# 페이지 클릭횟수 계산
page_click = math.ceil(film_num/4) - 1
print('페이지 클릭 횟수: ', page_click)


# 영화 데이터 수집
title_list = []
score_list = []
cast_list = []
audiance_list = []
main_sub_list = []

for i in range(page_click):    
    # 제목
    titles = driver.find_elements_by_css_selector('.this_text')
    temp_list = []
    for title in titles:
        temp_list.append(title.text)
        temp_list = [x for x in temp_list if x !='' and x != '상영중']
#     print(temp_list)    
    title_list = title_list + temp_list    
    
    # 배역
    cast_temp = []
    for t in range(1, 5):
        try:
            cast = driver.find_element_by_css_selector('#mflick > div:nth-child({}) > div > div > div:nth-child({}) > div.data_area > div > span'.format(i+1, t)).text[:2]
            cast_temp.append(cast)
        except:
            break
    cast_list = cast_list + cast_temp        
#     print(cast_temp)
    

    # 관객수
    score_temp = []
    for j in range(1, 5):
        try:
            score = driver.find_element_by_css_selector('#mflick > div:nth-child({}) > div > div > div:nth-child({}) > div.data_area > div > div.info > dl:nth-child(2)'.format(i+1, j)).text.split('\n')[1]
            score = score.replace('만','0000').replace(',','')
            if '.' in score:
                score = int(score.replace('.', ''))
                score = round(score*0.1)
            else:
                score = round(int(score))
            
            score_temp.append(score)
        except:
            break
    score_list = score_list + score_temp
#     print(score_temp, '\n')
    if len(score_temp) < 4:
        break
        
    # 필모 다음페이지 클릭
    driver.find_element_by_css_selector('.pg_next.on').click()
    time.sleep(2)

title_list = title_list[:len(score_list)]
cast_list = cast_list[:len(score_list)]

# print('\n', len(title_list))
# print(title_list)
# print(cast_list)
# print(score_list)

# 
df = pd.DataFrame(list(zip(title_list, cast_list, score_list)), columns = ['제목','배역', 'score'])
print('관객수 데이터가 있는 영화들만: ')
print(df)

# 주연 스코어 합
cast_main = df[(df['배역']=='주연')]
main_score = cast_main['score'].sum()

# 조연 스코어 합
cast_sub = df[(df['배역']=='조연')]
sub_score = cast_sub['score'].sum()

# 단역 스코어 합
cast_other = df[(df['배역']!='주연') & (df['배역']!='조연')]
cast_score = cast_other['score'].sum()

# 배우 흥행지수
actor_score_index = round((main_score*0.5 + sub_score*0.4 + cast_score*0.1) / len(df))
print('{} 배우 흥행지수: '.format(name), actor_score_index)
```
