## Crawling example

Hello, this is Nelchois.
Today I learned how to open and use the browser in automatically.
Here's my code.

```
from selenium import webdriver#웹 컨트롤러 #1 import
from selenium.webdriver.common.keys import Keys
browser=webdriver.Chrome()#2
browser.implicitly_wait(10)#화면이 다 뜰때까지 기다림
browser.get("http://www.google.com")
browser.implicitly_wait(10)
l=browser.find_element_by_xpath("/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input")
l.send_keys("뉴스")
browser.implicitly_wait(10)
l.send_keys(Keys.RETURN)
browser.implicitly_wait(10)
#time.sleep(2)
browser.execute_script("window.scrollTo(0, 500);")#스크롤을 500만큼 내림
browser.implicitly_wait(10)
l=browser.find_element_by_xpath('//*[@id="rso"]/div[2]/div/div/div[1]/div/a/h3')
l.click()
browser.implicitly_wait(10)
browser.execute_script("window.scrollTo(0, 500);")
```
Cautions: if we don't give short term, the server will block you, because they will consider you a bot. So you have to give some sec to deceive the server. In my code, ```browser.implicitly_wait(10)``` make some term.