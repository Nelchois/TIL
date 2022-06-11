Code for visualize and make score about each month.
```
data = pd.read_csv('preprocess_Eternal_Return.csv')
review_data = data.drop(['Unnamed: 0'], axis = 1)
review_data
review_data.isnull().values.any()
#open CSV, check null data

march_data = review_data[review_data['date'].str.contains('March')]
march_data
#Get data writed in march from dataframe 

march_data_0 = march_data[march_data['recommend'] == 0]
march_data_1 = march_data[march_data['recommend'] == 1]
march_recommend_rate = len(march_data_1)/len(march_data) * 100
print(f'3월의 점수 : {march_recommend_rate : .2f} 점')
#make score in March

april_data = review_data[review_data['date'].str.contains('April')]
april_data
april_data_0 = april_data[april_data['recommend'] == 0]
april_data_1 = april_data[april_data['recommend'] == 1]
april_recommend_rate = len(april_data_1)/len(april_data) * 100
print(f'4월의 점수 : {april_recommend_rate : .2f} 점')
#make score in April

may_data = review_data[review_data['date'].str.contains('May')]
may_data
may_data_0 = may_data[may_data['recommend'] == 0]
may_data_1 = may_data[may_data['recommend'] == 1]
may_recommend_rate = len(may_data_1)/len(may_data) * 100
print(f'5월의 점수 : {may_recommend_rate : .2f} 점')
#make score in May
```
