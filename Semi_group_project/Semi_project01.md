Hello, this is Nelchois.
This code is crawling code that get some data of book in library page.
```
import pandas as pd
import numpy as np

# 라이브러리 import
# 라이브러리 : 필요한 도구
from selenium import webdriver  # 라이브러리(모듈) 가져오라
from selenium.webdriver import ActionChains as AC
import chromedriver_autoinstaller
from tqdm import tqdm
from tqdm import tqdm_notebook
import re
from time import sleep
import time

# 워닝 무시
import warnings
warnings.filterwarnings('ignore')

url = "https://ebook.pknu.ac.kr/ebook/?code=&mode=&sort=&cate_id=&page_num=1&view=50#list_tab"

chrome_path = chromedriver_autoinstaller.install()
driver = webdriver.Chrome(chrome_path)
driver.get(url)

# 책 종류 크롤링
overlays = "#booklist > ul > li:nth-child(1) > div.info > span > span"                                
kind = driver.find_element_by_css_selector(overlays)          
kind = kind.text
kind

# 주소 크롤링
overlays = "#booklist > ul > li:nth-child(1) > div.info > span > a"                                 
add = driver.find_element_by_css_selector(overlays)          
add = add.get_attribute('href') 
print(add)

# 작가 크롤링
overlays = "#booklist > ul > li:nth-child(1) > div.info > ul > li:nth-child(1)"
writer = driver.find_element_by_css_selector(overlays)         
writer = writer.text
writer

# 렌탈 정보 크롤링
overlays = "#booklist > ul > li:nth-child(1) > div.info > div"                                 
rent = driver.find_elements_by_css_selector(overlays)    
rent = rent[0].text

page = 10 # 크롤링할 페이지 수 
add_list = [] # 주소 크롤링 데이터를 담을 그릇
chrome_path = chromedriver_autoinstaller.install()
options = webdriver.ChromeOptions()
options.add_argument('headless')
options.add_argument('window-size=1920x1080')
options.add_argument("disable-gpu")
driver = webdriver.Chrome(chrome_path, chrome_options=options)

for j in range(1,page+1):
    
    dict = {}    # 전체 크롤링 데이터를 담을 그릇
        
    # 글 띄우기
    url = f"https://ebook.pknu.ac.kr/ebook/?code=&mode=&sort=&cate_id=&page_num={j}&view=50#list_tab"      
    driver.get(url)
    # 수집할 글 갯수 정하기
    number = 50
    
    # 수집한 url 돌면서 데이터 수집
    for i in tqdm_notebook(range(1, number+1)):

        # 크롤링
        # 예외 처리
        try : 
            target_info = {}  # 개별 내용을 담을 딕셔너리 생성

            # 책 종류 크롤링
            overlays = f"#booklist > ul > li:nth-child({i}) > div.info > span > span"      
            kind = driver.find_element_by_css_selector(overlays)          
            kind = kind.text

            # 책 제목 크롤링
            overlays = f"#booklist > ul > li:nth-child({i}) > div.info > span > a"                                 
            tit = driver.find_element_by_css_selector(overlays)          
            tit = tit.text

            # 주소 크롤링
            overlays = f"#booklist > ul > li:nth-child({i}) > div.info > span > a"                                 
            add = driver.find_element_by_css_selector(overlays)          
            add = add.get_attribute('href')
            add_list.append(add)

            # 작가 크롤링
            overlays = f"#booklist > ul > li:nth-child({i}) > div.info > ul > li:nth-child(1)"
            writer = driver.find_element_by_css_selector(overlays)         
            writer = writer.text

            # 렌탈 정보 크롤링
            overlays = f"#booklist > ul > li:nth-child({i}) > div.info > div"                                 
            rent = driver.find_elements_by_css_selector(overlays)    
            rent = rent[0].text                       

            # 글 하나는 target_info라는 딕셔너리에 담기게 되고,
            target_info['kind'] = kind
            target_info['tit'] = tit
            target_info['writer'] = writer
            target_info['rent'] = rent

            # 각각의 글은 dict라는 딕셔너리에 담기게 됩니다.
            dict[i] = target_info

            # 크롤링이 성공하면 글 제목을 출력하게 되고,
            print(i, tit)


        # 에러나면 현재 크롬창을 닫고 다음 글(i+1)로 이동합니다.
        except:
            print("에러",i, tit)
            time.sleep(1)
            continue

    # 판다스 데이터프레임으로 만들기   
    globals()['result_df'+str(j)] = pd.DataFrame.from_dict(dict, 'index')
#get text from url pages

#access to webpage by url and take some part of data(name, category, writer and #info)

body_list = []
chrome_path = chromedriver_autoinstaller.install()
options = webdriver.ChromeOptions()
options.add_argument('headless')
options.add_argument('window-size=1920x1080')
options.add_argument("disable-gpu")
options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')
driver = webdriver.Chrome(chrome_path, chrome_options=options)
for i in tqdm_notebook(add_list[1000:2000]):
    try :
        driver.get(i) 
        #내용 크롤링
        overlays = "#bookinfo"                                 
        body = driver.find_element_by_css_selector(overlays)          
        body = body.text
        body_list.append(body)
         
    except:
        print("에러",i)
        body_list.append("non") 
        continue        
driver.close()
#the summary of book is too long that average length of data is over than 1,000
#so, we make code seperately. 