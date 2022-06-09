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
#make score in march

```
