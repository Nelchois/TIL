This is Nelchois, now days I use Jupyter notebook. Because I learned about how can I use numpy and pandas. It's easy to check line to line moreover there is immediate feedback if a code line is running.

## Data

feature is characteristic of data.
It's most important factor that build model.
model usually express to fucntion(-> y = ax + b).
In this fucntion, y is dependent variable that result of model and x is independent variable that input of model.
a, b are important things, we call that weight value, which find as algorithm.
---
Data_table is expressed data in graph(excel).
Data_instance is one data line in excel.
---
continuous data is continued value like temperature, speed, mean, dispersion.
This data also has scale.
discrete data is not continued value like text, address number, ... ect.
This data don't have scale and separated by labels. 

## Numpy
1. Numpy is module that make us easy to using matrix like this.
```
import numpy as np
t_arr=np.array([1,4,5,8],float) #We can give data type
t_arr
``` 
Also they give to us that data's information.
```t_3=[[[1,2,5,8],[4,5,6,7],[6,7,8,9]],[[1,2,5,8],[4,5,6,7],[6,7,8,9]],[[1,2,5,8],[4,5,6,7],[6,7,8,9]]]
np.array(t_3).shape
```
Then they return us (3,3,4) which means t_3 have axis = 2. 
Don't confuse axis is started at 0 so 2 means it's a 3D matrix.
2. Axis is most important thing in Numpy. It's a standard of matrix calculation in Numpy. 
Here's some example codes
``` 
import numpy as np
t_a=np.arange(1,11)
print(t_a.sum())
t_a2=np.arange(1,13).reshape(3,4)
print(t_a2)
t_a2.sum(axis=0) 
```
The final code(t_a2.sum(axis=0) is return the sum of line that array([15,18,21,24]). However, if you give 1 in axis then it will return the sum of row in that array([10,26,42]). So axis=0 means proximate dimension that similar to index. 
3. Reshape and Data type
In numpy array we can use any type of data like this.
```
import numpy as np
x=np.arange(3,10)
print(x>5)
```
Then it will return array([False, False, False, True, True, True, True]) that makes us easy to check the data array. Also we can write the code 
```
import numpy as np
x=np.arange(3,10)
print(x>5).all()
```
This code will return just False, because at least one of the result is false. Like this, ```(x>5).any()``` means at least one of the result is true, so result is True.

4. Calculation of array
In matrix, the operation is performed on each element of the matrix.
```
x=array([1,2,3],
        [4,5,6])
x+x = array([[2,4,6],
            [8,10,12]])
x**x= array([[1,4,27],
       [256,3125,46656]],dtype=int32)
x==x = array([[True,True,True],
       [True,True,True]])
```

## Pandas
* Join
We can connect data between pandas DataFrame.
There are 4 types of join :'inner join', 'full join', 'left join', 'right join'
inner join : If two tables have same key, merge that row's exist values.
full join : In two tables merge each row, if there are same keys combine that row. However, if there are no keys just let the exist data.
left join : Based on left table in two tables merge the values in same keys if that key is not in right table, delete that row.
right join : The opposite of left join.  

Practice code
```
b=webdriver.Chrome()
b.implicitly_wait(10)
b.get("http://naver.com")
b.find_element_by_xpath('//*[@id="query"]').send_keys("금융\n")
b.implicitly_wait(10)
b.find_element_by_xpath('//*[@id="web_1"]/div/div[2]/div[2]/a').click()
b.implicitly_wait(10)
b.switch_to.window(b.window_handles[1])
b.find_element_by_xpath('//*[@id="menu"]/ul/li[4]/a/span').click()
b.implicitly_wait(10)
b.find_element_by_xpath('//*[@id="tab_section"]/ul/li[2]/a/span').click()
b.implicitly_wait(10)
b.switch_to.frame('frame_ex2')
def f(x): return x.text.strip().replace("\n", "").replace("\t", "")
title=''
l = []
for page in range(1,5):
    b.find_element_by_xpath(f'/html/body/div/div/a[{page}]').click()
    print(f"{page}페이지 진행 중")
    html = b.page_source
    s=BeautifulSoup(html, 'html.parser')
    data = s.select_one('tbody').select('tr')
    title = s.select_one('thead').select("th")
    for i in data:
        l.append(map(f, i.select('td')))
b.quit()
``` 
This code make DataFrame with webpage crawling.

* Data grouping
We can make a group with pandas and numpy.
Here's a DataFrame by pandas.
Grouping EX1
```
import numpy as np
import pandas as pd

ipl_data = {'Team': ['Riders', 'Riders', 'Devils', 'Devils', 'Kings','kings', 'Kings', 'Kings', 'Riders', 'Royals', 'Royals', 'Riders'],
'Rank': [1, 2, 2, 3, 3,4 ,1 ,1,2 , 4,1,2],
'Year': [2014, 2015, 2014, 2015, 2014, 2015, 2016, 2017, 2016, 2014, 2015, 2017],
'Points':[876,789,863,673,741,812,756,788,694,701,804,690]}
df=pd.DataFrame(ipl_data)
```
If we write .groupby like this ```c_df=df.groupby(["Team","Year"])["Points"].sum()``` then it show us Points sorted in Team and Year. 

Grouping EX2
```
import numpy as np
import pandas as pd

ipl_data = {'Team': ['Riders', 'Riders', 'Devils', 'Devils', 'Kings','kings', 'Kings', 'Kings', 'Riders', 'Royals', 'Royals', 'Riders'],
'Rank': [1, 2, 2, 3, 3,4 ,1 ,1,2 , 4,1,2],
'Year': [2014, 2015, 2014, 2015, 2014, 2015, 2016, 2017, 2016, 2014, 2015, 2017],
'Points':[876,789,863,673,741,812,756,788,694,701,804,690]}
df=pd.DataFrame(ipl_data)
c_df=df.groupby(["Team","Year"])["Points"].sum()
```
