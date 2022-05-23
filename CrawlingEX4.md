Question, Get some datasets, using webpage crawling and make DataFrame with pandas.
### crawling
```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup
import time
browser=webdriver.Chrome()
wait = browser.implicitly_wait(10)
browser.get("http://www.naver.com")
wait
l=browser.find_element_by_xpath('//*[@id="query"]')
l.send_keys("네이버영화")
wait
l.send_keys(Keys.RETURN)
wait
l=browser.find_element_by_xpath('//*[@id="main_pack"]/section[1]/div/div/div[1]/div/div[2]/a/mark')
l.click()
browser.switch_to.window(browser.window_handles[1])
wait
I = browser.find_element_by_xpath('//*[@id="scrollbar"]/div[1]/div/div/ul/li[4]/a')
I.click()
browser.execute_script("window.scrollTo(0, 500);")
def f(x): return x.text.strip().replace("\n", "").replace("\t", "")
review_data = []
for page in range(1,10):
    html = browser.page_source
    soup = BeautifulSoup(html,'html.parser')
    reviews = soup.find_all("td",{"class":"title"})
    browser.find_element_by_xpath(f'//*[@id="old_content"]/div[2]/div/a[{page}]').click()
    time.sleep(3)
    for review in reviews:
        sentence = review.find("a",{"class":"report"}).get("onclick").split("', '")[2]
        if sentence != "":
            movie = review.find("a",{"class":"movie color_b"}).get_text()
            score = review.find("em").get_text()
            review_data.append([movie,sentence,int(score)])
        time.sleep(2)
browser.quit()
```
This code is crawling from movie_review page in naver(south_korea).
Warning, if you don't use ```time.sleep``` or ```browser.implicity_wait``` then webpages will kick or ban you, because they tought access by bot.  

### DataFrame
```
import pandas as pd
import numpy as np
columns_name = ['movie', 'review', 'rating']
movie_dataframe = pd.DataFrame(review_data, columns = columns_name)
```
