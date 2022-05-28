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
Search the actor name in potal website, get some information of filmography and awards.
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

Here is example of Sample2 that crawling for Instagram images.
However, I don't recommend using this code for follow reason. 
First, Instagram changes it's structure regularly, so its hard to use sample code.
Second, it checks crawler or bot hard, so you will ban easily.
```
# 데이터 수집할 키워드 지정
keyword = "역삼맛집"
len_insta = 20  # 몇 개의 게시글을 수집할 것인가

# url에 검색 'keyword' 입력
url = "https://www.instagram.com/explore/tags/{}/".format(keyword)

chrome_path = chromedriver_autoinstaller.install()
driver = webdriver.Chrome(chrome_path)
driver.get(url)
 
manual login

dict = {}  # 전체 게시글을 담을 그릇

# 첫번째 사진 클릭
CSS_tran=".eLAPa"
elements = driver.find_elements_by_css_selector(CSS_tran)   # 사진 클릭
elements[0].click()
time.sleep(2)

# 크롤링 시작
for j in tqdm_notebook(range(0, len_insta)):    

    target_info = {}                                            # 사진별 데이터를 담을 딕셔너리 생성

    try:    # 크롤링을 시도해라.
        # 사진(pic) 크롤링
        overlays1 = ".eLAPa.RzuR0 .FFVAD"                   # 사진창 속 사진   
        img = driver.find_element_by_css_selector(overlays1)    # 사진 선택
        pic = img.get_attribute('src')                          # 사진 url 크롤링 완료
        target_info['picture'] = pic

        # 날짜(date) 크롤링
        overlays2 = "._1o9PC"                # 날짜 지정
        datum2 = driver.find_element_by_css_selector(overlays2)     # 날짜 선택
        date = datum2.get_attribute('title')
        target_info['date'] = date

        # 좋아요(like) 크롤링
        overlays3 = "._7UhW9.xLCgt.qyrsm.KV-D4.fDxYl.T0kll"                                        # 리뷰창 속 날짜
        datum3 = driver.find_element_by_css_selector(overlays3)     # 리뷰 선택
        like = datum3.text                                          # 좋아요 크롤링 완료
        target_info['like'] = like

        # 해시태그(tag) 크롤링
        overlays4 = ".xil3i"                                         
        datum3 = driver.find_elements_by_css_selector(overlays4)    # 태그 선택
        tag_list = []
        for i in range(len(datum3)):
            tag_list.append(datum3[i].text)
        target_info['tag'] = tag_list

        dict[j] = target_info            # 토탈 딕셔너리로 만들기

        print(j, tag_list)

        # 다음장 클릭
        CSS_tran2="body > div.RnEpo._Yhr4 > div.Z2Inc._7c9RR > div > div.l8mY4.feth3 > button > div > span > svg"             # 다음 버튼 정의
        tran_button2 = driver.find_element_by_css_selector(CSS_tran2)  # 다음 버튼 find
        AC(driver).move_to_element(tran_button2).click().perform()     # 다음 버튼 클릭
        time.sleep(3)

    except:  # 에러가 나면 다음장을 클릭해라
        # 다음장 클릭
        CSS_tran2="body > div.RnEpo._Yhr4 > div.Z2Inc._7c9RR > div > div.l8mY4.feth3 > button > div > span > svg"             # 다음 버튼 정의
        tran_button2 = driver.find_element_by_css_selector(CSS_tran2)  # 다음 버튼 find
        AC(driver).move_to_element(tran_button2).click().perform()     # 다음 버튼 클릭

        time.sleep(3)

print(dict)
```
