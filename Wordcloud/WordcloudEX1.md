### First EX
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

### Second EX
```
st_w = set(STOPWORDS)
st_w.add('파이썬')
wc = WordCloud(font_path = f_path, background_color = 'white', stopwords = st_w).generate(text)
text2 = '나는 파이썬을 공부중 입니다. 파이썬으로 새로운 문장을 만드는 공부 중 입니다.'
ck = {'파이썬': 3, '공부': 1, '나는': 2}
wc2 = WordCloud(font_path = f_path)
wc2 = wc.generate_from_text(text2)#횟수만 고려하여 강조
wc2 = wc2.generate_from_frequencies(ck)#문자의 우선순위를 고려하여 출력
plt.figure(figsize = (10, 10))
plt.imshow(wc2, interpolation = 'lanczos')
plt.axis('off')
plt.show()
```

### Third EX(black background)
```
from collections import Counter
text_list = ['안녕', '안녕', '안녕', '안녕', '안녕', '안녕', '나는', '나는', '나는', '파이썬', '파이썬']
combine = Counter(text_list)
word_cloud3 = WordCloud(font_path = f_path)
generate_data = word_cloud3.generate_from_frequencies(combine)
plt.figure()
plt.imshow(generate_data)
plt.axis('off')
plt.show()
```