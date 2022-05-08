## Crawling example
Hello, there! Today I practice about print data in order.
Here's my code.
```
import requests
from bs4 import BeautifulSoup
url = 'https://finance.naver.com/sise/sise_rise.naver'
r=requests.get(url)
soup=BeautifulSoup(r.text,"html.parser")
data=soup.select("td")
def f(n,x):
    for i in range(x):
        n=n.next_sibling
    return n.text
for i in data:
    if i.a:
        print(f"종목명:{i.a.text},현재가:{f(i,2)},PER{f(i,18)}")
```
Same as yesterday, using Beautifulsoup. However, print is different that using f-string. 