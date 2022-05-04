Today, I practice web crawling and DataFrame.
```
b.find_element_by_xpath('//*[@id="web_1"]/div/div[2]/div[2]/a').click()
b.implicitly_wait(10)
b.switch_to.window(b.window_handles[1])
b.find_element_by_xpath('//*[@id="menu"]/ul/li[4]/a/span').click()
b.implicitly_wait(10)
b.execute_script("window.scrollTo(0, 500)")
b.implicitly_wait(10)
b.find_element_by_xpath('//*[@id="tab_section"]/ul/li[2]/a').click()
iframe = b.find_element_by_id('frame_ex2')
b.switch_to.frame(iframe)
t=open("finance.csv","w",encoding="utf-8-sig",newline="")
writer=csv.writer(t)
writer.writerow(['통화명','심볼','현재가','전일대비','등락률'])
data_l=[]
def f(n,x):
    for i in range(x):
        n=n.next_sibling
    return n.text
for i in range(2,5):
    b.execute_script("window.scrollTo(0, document.body.scrollHeight)")
    html=b.page_source
    b.find_element_by_css_selector(f'body > div > div > a:nth-child({i})').click()
    b.implicitly_wait(10)
    s=BeautifulSoup(html,"html.parser")
    data=s.select_one('tbody')
    data_tr=data.select('tr')
    for i in data_tr:
        data_l.append({'통화명':i.td.text.strip(),'심볼':f(i.td,2).strip(),'현재가':f(i.td,4).strip(),'전일대비':f(i.td,6).strip(),'등락률':f(i.td,8).replace('\t','').replace('\n','')})
        b.implicitly_wait(10)
writer.writerow(data_l)
obj=DataFrame(data_l,columns=['통화명','심볼','현재가','전일대비','등락률'])
final_obj=obj.set_index('통화명')
final_obj.to_csv("finance_data.csv",encoding='utf-8-sig')
```

