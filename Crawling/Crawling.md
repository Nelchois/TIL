# Crawling
Crawling is technology that extract information in web pages. 
There are two variations on it, crawling and scraping.

* Crawling is using in search engines like google. 
It use crawler(bot) to extract information in web pages and make database based on it. 
Bot can visit automaticly to web pages like news, blog and get contents and who write it.

* Scraping is one of the sub-concepts in crawling, however it just get information specific page by connecting url.

### Extract data from web page
Today I learned about how to extract data from a web page.
It's pretty difficult for me, because I never learned SQL and HTML before, so I tried to solve the small question in my textbook. Here's my code. 

```
    from html import unescape
def 추출(url):
    f = urlopen(url)
    encoding = f.info().get_content_charset(failobj="UTF-8")
    html = f.read().decode(encoding)
    return html
def 정규화(html):
    data=[]
    for i in re.findall(r'<td class="left"><a.*?</td>', html, re.DOTALL):
        url = re.search(r'<a href="(.*)">',i).group(1)
        url = "http://www.hanbit.co.kr"+url
        title = re.sub(r'<.*?>','',i)
        unescape(title)
        data.append({'url':url,"title":title})
    return data
def 저장(db,data):
    conn = sqlite3.connect(db)
    c=conn.cursor()
    c.execute('DROP TABLE IF EXISTS data')
    c.execute('''
            CREATE TABLE data  (
                title text,
                url text
            )
        ''')
    c.executemany('INSERT INTO data VALUES (:title, :url)',data)
    conn.commit()
    conn.close()
def 출력(db):
    conn = sqlite3.connect(db)
    c = conn.cursor()
    c.execute('SELECT * FROM data')
    for i in c.fetchall():
        print(i)
    conn.close()
if __name__=="__main__":
    html = 추출("http://www.hanbit.co.kr/store/books/full_book_list.html")
    data = 정규화(html)
    저장("data.db",data)
    출력("data.db")
```