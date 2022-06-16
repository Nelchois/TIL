Code for visualize and make score about each month.
```
data = pd.read_csv('preprocess_Eternal_Return.csv')
review_data = data.drop(['Unnamed: 0'], axis = 1)
review_data
review_data.isnull().values.any()
#open CSV, check null data

March_data = review_data[review_data['date'].str.contains('March')]
March_data
#Get data writed in March from dataframe 

March_data_0 = March_data[march_data['recommend'] == 0]
March_data_1 = march_data[march_data['recommend'] == 1]
March_recommend_rate = len(March_data_1)/len(March_data) * 100
print(f'3월의 점수 : {March_recommend_rate : .2f} 점')
#make score in March

April_data = review_data[review_data['date'].str.contains('April')]
April_data
April_data_0 = April_data[April_data['recommend'] == 0]
April_data_1 = April_data[April_data['recommend'] == 1]
April_recommend_rate = len(April_data_1)/len(April_data) * 100
print(f'4월의 점수 : {April_recommend_rate : .2f} 점')
#make score in April

May_data = review_data[review_data['date'].str.contains('May')]
May_data
May_data_0 = May_data[May_data['recommend'] == 0]
May_data_1 = May_data[May_data['recommend'] == 1]
May_recommend_rate = len(May_data_1)/len(May_data) * 100
print(f'5월의 점수 : {May_recommend_rate : .2f} 점')
#make score in May

x = np.arange(3)
rate = [March_recommend_rate, April_recommend_rate, May_recommend_rate]
month = ['March', 'April', 'May']
plt.bar(x, rate, color = ['r', 'g', 'b'])
plt.xticks(x, month)
plt.xlabel('month', fontsize = 12)
plt.ylabel('score', fontsize = 12)
plt.title('Monthly review score', fontsize = 20)
#make graph about Montly score(March, April, May)

under100_data = review_data.loc[review_data['playtime'] <= 100 ]
under300_data = review_data.loc[(review_data['playtime'] > 100) & (review_data['playtime'] <= 300)]
under500_data = review_data.loc[(review_data['playtime'] > 300) & (review_data['playtime'] <= 500)]
over500_data = review_data.loc[review_data['playtime'] >500 ]
#classify the dataframe based on 'playtime'

len(under100_data), len(under300_data), len(under500_data), len(over500_data)
#check the lengh of each data

under100_1 = under100_data[under100_data['recommend'] == 1]
under100_score = len(under100_1)/len(under100_data) * 100
print(f'100시간 이하 점수 : {under100_score : .2f} 점')

under300_1 = under300_data[under300_data['recommend'] == 1]
under300_score = len(under300_1)/len(under300_data) * 100
print(f'100시간 초과 300시간 이하 점수 : {under300_score : .2f} 점')

under500_1 = under500_data[under500_data['recommend'] == 1]
under500_score = len(under500_1)/len(under500_data) * 100
print(f'300시간 초과 500시간 이하 점수 : {under500_score : .2f} 점')

over500_1 = over500_data[over500_data['recommend'] == 1]
over500_score = len(over500_1)/len(over500_data) * 100
print(f'500시간 초과 점수 : {over500_score : .2f} 점')
#print score from each group

x = np.arange(4)
rate = [under100_score, under300_score, under500_score, over500_score]
playtime = ['under_100', '100 to 300', '300 to 500', 'over_500' ]
plt.bar(x, rate, color = ['r', 'g', 'b', 'y'], alpha = 0.7)
plt.xticks(x, playtime)
plt.xlabel('playtime', fontsize = 12)
plt.ylabel('score', fontsize = 12)
plt.title('score in playtime', fontsize = 20)
#print graph of score

import seaborn as sns
import re
import urllib.request
from konlpy.tag import Okt
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
from wordcloud import WordCloud,STOPWORDS
from hanspell import spell_checker
from tqdm import tqdm
from matplotlib import font_manager, rc
font_path = r'C:\Windows\Fonts\GULIM.TTC'
font = font_manager.FontProperties(fname=font_path).get_name()
rc('font', family=font)
from pykospacing import Spacing
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from collections import Counter
#import package for morphs

Learning_dataframe = review_data.rename(columns = {'recommend':'y', 'review' : 'X'})
Learning_dataframe.isnull().values.any()
stop_word=set(['은','는','이','가','를','들','에게','의','을','도','으로','만','라서','하다'])
Learning_dataframe = Learning_dataframe.drop_duplicates(subset = ['X'])
Learning_dataframe['clean_X'] = Learning_dataframe.X.str.replace('[^ㄱ-ㅎㅏ-ㅣ가-힣 ]','')
okt=Okt()
X_data=[] 
for i in tqdm(Learning_dataframe['clean_X']): 
    token_data = okt.nouns(i) 
    end_data=[word for word in token_data if not word in stop_word]#불용어 처리 
    X_data.append(' '.join(end_data))
#set stopwords and tokenize the dataframe

for i in X_data:
    if len(i) < 2:
        X_data.remove(i)
sort_X = list(set(X_data))
sort_X.remove('')
split_sort_X = []
for i in sort_X:
    split_i = i.split()
    for _ in range(len(split_i)):
        a = split_i.pop()
        split_sort_X.append(a)
# Using 'set' in line 116, there are many overlapping about '', so 'set' remove 
#all of '' except one

split_sort_X
count = Counter(split_sort_X)
words = dict(count.most_common())
words
sort_text = ' '.join(i for i in split_sort_X)
sort_text

stopwords = set(STOPWORDS)
stopwords.update(['게임', '겜', '좀', '때', '것'])
#set stopwords for wordcloud
wc = WordCloud(stopwords=stopwords, font_path=r'C:\Users\student\AppData\Local\Microsoft\Windows\Fonts\NanumGothic.ttf', background_color = 'white', max_words= 2000).generate_from_text(sort_text)
plt.imshow(wc)
#open the font and make wordcloud
```
