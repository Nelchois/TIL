## Scraping example
Hey there, this is Nelchois.
Today, I learned how to use requests method and Beautifulsoup in bs4.
Here's my codes about scraping.
```
import requests
from bs4 import BeautifulSoup
url="https://media.naver.com/press/044"
r=requests.get(url)
soup=BeautifulSoup(r.text,"html.parser")
data=soup.select('span.press_edit_news_title')
for i in data:
    t=i.get_text()
    print(t)
```
```
import requests
from bs4 import BeautifulSoup
url="https://finance.naver.com/sise/sise_rise.naver"
r=requests.get(url)
soup=BeautifulSoup(r.text,"html.parser")
data=soup.select('a.tltle')
for i in data:
    print(i.text)
 ```
There are some notes for these codes. Check the spelling about Markup
for example, compare line 11 with 22, tag in line 11 is 'title' however
tag in line 22 is 'tltle' not 'title'. I wasted 30 minutes on this. 