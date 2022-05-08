# Matplotlib
```
import matplotlib.pyplot as plt
x=[1,2,3]
y=[-2,-4,-8]
y1=[1,3,7]
plt.plot(x,y) # send the data to matplot
plt.plot(x,y1)
plt.show()

import numpy as np # we can also use the numpy data and methods.
X_1=range(100)
Y_1=[np.cos(i) for i in X_1]

X_2=range(100)
Y_2=[np.sin(i) for i in X_1]
plt.plot(X_1,Y_1)
plt.plot(X_2,Y_2)
```
If you want to show more than one graph that you can use ```subplots```
```
fig=plt.figure()
fig.set_size_inches(10,10)
import numpy as np
X_1=range(100)
Y_1=[np.cos(i) for i in X_1]
X_2=range(100)
Y_2=[np.sin(i) for i in X_2]
ax_1= fig.add_subplot(1,2,1)
ax_2= fig.add_subplot(1,2,2)
ax_1.plot(X_1,Y_1)
ax_2.plot(X_2,Y_2)
plt.show()
```
There are some coordinate after ```add_subplot```, in this code (1,2,1), (1,2,2).
Using these coordinate, we can control the orders of graph.

## Visualize Wordcloud
```
from wordcloud import WordCloud, STOPWORDS
import matplotlib.pyplot as plt
f_path='C:\Windows\Fonts\malgun.ttf'
'''
WordCloud(max_words = "최대 수용 단어 개수", 
          background_color = '색상',
          font_path = '폰트설정(파일 경로)',
          width = '넓이', 
          height = '높이')
'''
wc = WordCloud(font_path = f_path, background_color = 'white')
text = '파이썬 코로나 파이썬 삼성 멀티캠 워드 워드클라우드 워드 삼성 나는 나 이런 이상 파이썬 코로나 데이터 자료 파이썬'
wc = wc.generate(text)
plt.figure(figsize = (10, 10))
plt.imshow(wc, interpolation = 'lanczos')
plt.axis('off')
plt.show()
```
