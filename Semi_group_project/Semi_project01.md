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
```
#access to webpage by url and take some part of data(name, category, writer and info)