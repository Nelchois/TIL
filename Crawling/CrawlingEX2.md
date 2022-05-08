Hey, there! this is Nelchois.
Today I tried to crawl the webpage automatically and saved it in CSV file. 
Saving in csv file we should be encoding 'utf-t-sig' that if you aren't use English.
```
from selenium import webdriver
from bs4 import BeautifulSoup
import csv
import time
import warnings#무시하기
warnings.filterwarnings('ignore')
b=webdriver.Chrome()
b.maximize_window()#크기설정
b.get("https://naver.com")
b.implicitly_wait(10)
in_b=b.find_element_by_xpath('//*[@id="query"]')
in_b.send_keys("암호화폐\n")
b.implicitly_wait(10)
b.find_element_by_xpath('//*[@id="lnb"]/div[1]/div/ul/li[2]/a').click()
b.implicitly_wait(10)
f=open("news.csv","w",encoding="utf-8-sig",newline="")
writer=csv.writer(f)
data_l=[]
for i in range(2,6):
    b.execute_script("window.scrollTo(0, document.body.scrollHeight)")
    b.implicitly_wait(10)
    html=b.page_source
    b.find_element_by_xpath(f'//*[@id="main_pack"]/div[2]/div/div/a[{i}]').click()
    b.implicitly_wait(10)
    s=BeautifulSoup(html,'html.parser')
    data=s.select('a.news_tit')
    for i in data:
        data_l.append([i.text,i.next_sibling.next_sibling.text.strip()])
        b.implicitly_wait(10)
writer.writerows(data_l)
f.close()
f=open("news.csv","r",encoding="utf-8-sig",newline="")
reader=csv.reader(f)
for i in reader:
    print(i[0],i[1])
``` 
Also, you have to check the time term like ```time.sleep()``` or 
```.imlicitly_wait()```