## Scraping example2
Hey, there this is Nelchois
I tried to make some crawling codes with using sqlite3.
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

import sqlite3
data_db=[]
for i in data:
    if i.a:
        data_db += [{"종목명":i.a.text,"현재가":f(i,2),"전일비":f(i,4).strip(),"등락률":f(i,6).strip(),"거래량":f(i,8),"매수호가":f(i,10),"매도호가":f(i,12),"매수총잔량":f(i,14),"매도총잔량":f(i,16),"PER":f(i,18),"ROE":f(i,20)}]
db = "data_db.db"


def 저장(db, data_db):
    conn = sqlite3.connect(db)
    c = conn.cursor()
    c.execute('DROP TABLE IF EXISTS data_db')
    c.execute('''
                CREATE TABLE data_db (
                        종목명 text,
                        현재가 text,
                        전일비 text,
                        등락률 text,
                        거래량 text,
                        매수호가 text,
                        매도호가 text,
                        매수총잔량 text,
                        매도총잔량 text,
                        PER text,
                        ROE text
                     )
                ''')
    c.executemany('INSERT INTO data_db VALUES (:종목명, :현재가, :전일비, :등락률, :거래량, :매수호가, :매도호가, :매수총잔량, :매도총잔량, :PER, :ROE)', (data_db))
    conn.commit()
    conn.close()


def 출력(db):
    conn = sqlite3.connect(db)
    c = conn.cursor()
    c.execute('SELECT * FROM data_db')
    for i in c.fetchall():
        print(i)


저장(db, data_db)
출력(db)
```