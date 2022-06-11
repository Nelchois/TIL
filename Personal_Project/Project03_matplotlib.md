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
```
