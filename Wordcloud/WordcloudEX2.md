### WordcloudEX2
Question open txt file and analyze that data, finally make wordcloud graph.
```
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from collections import Counter
from konlpy.tag import Okt
with open('ex1_data.txt', 'r', encoding= 'utf-8') as f:
    text = f.read()
text_analyze = Okt()
text_data = text_analyze.nouns(text)
text_data2 = [i for i in text_data if len(i) > 1]
text_data3 = Counter(text_data2)
text_data3
Law_wc = WordCloud(font_path = 'C:\Windows\Fonts\malgun.ttf', max_words = 100, max_font_size = 180, background_color = 'white')
generate_data = Law_wc.generate_from_frequencies(text_data3)
plt.figure()
plt.imshow(generate_data)
plt.axis('off')
plt.show()
```