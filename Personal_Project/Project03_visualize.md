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
```
