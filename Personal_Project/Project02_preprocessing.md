This code is connected to Project01.
```
import pandas as pd
import numpy as np

using_data = pd.read_csv('steam_Eternal_Return_review.csv', encoding = 'utf-8')
using_data
#Open CSV for DataFrame.

data.replace({'recommend': 'Not Recommended'}, 0, inplace = True)
data.replace({'recommend': 'Recommended'}, 1, inplace = True)
#In DataFrame, recommend and not recommend are to complex, so I change recommend #to 1 and not recommend to 0.

using_data.columns
using_data.drop(['Unnamed: 0'], axis = 1, inplace = True)
#Check the columns and drop the useless columns.

for i in range(1020):
    using_data['playtime'][i] = using_data['playtime'][i].replace('hrs on record', '')
#We should delete 'hrs' for change data type.

for i in range(1020):
    using_data['review'][i] = using_data['review'][i].replace('\t', '')
    using_data['review'][i] = using_data['review'][i].replace('Early Access Review', '')    
    using_data['review'][i] = using_data['review'][i].replace('Product received for free', '')
#Replace the useless data in review.

for i in range(1020):
    replace_review = using_data['review'][i].replace(' ', '')
    using_data['review'][i] = replace_review.split()
#Split the review for make new DataFrame.

for i in using_data['review']:
    print(i)
#Check the reviews in DataFrame

review_date = []
review_document = []
for i in range(1020):
    try:
        date = using_data['review'].iloc[i][0]
        review_date.append(date)
        document = using_data['review'].iloc[i][1]
        review_document.append(document)
    except:
        print(f'{i}번째 리뷰')
        continue
#This code make review list, however there are some problem to access the data by index, so check that line using try, except.

drop_data = using_data.drop([430, 940], axis = 0)
sorted_data = drop_data.reset_index(drop = True)
#Drop unaccessible data line and sort the index in dataframe.

review_date = []
review_document = []
for i in range(1018):
    try:
        date = sorted_data['review'].iloc[i][0]
        review_date.append(date)
        document = sorted_data['review'].iloc[i][1]
        review_document.append(document)
    except:
        print(f'{i}번째 행')
        date = sorted_data['review'].loc[i][0]
        review_date.append(date)
        document = sorted_data['review'].loc[i][1]
        review_document.append(document)
        continue
#This code make date and document list by data from dataframe.
#Also, some line get some error that access by index, so in except, using loc.

print(len(review_date), len(review_document))
#Check the length of data.

se_review_date = pd.Series(review_date)
se_review_document = pd.Series(review_document)
column_drop_data = sorted_data.drop('review', axis = 1)
column_drop_data['review'] = se_review_document
column_drop_data['date'] = se_review_date
#Make new data frame with series and sorted_data


for i in range(1018):
    if type(column_drop_data['playtime'][i]) == str:
        column_drop_data['playtime'][i] = column_drop_data['playtime'][i].replace(',', '')
#There is mark in 'playtime' which ',' so, we have to remove that before change #data type.
for i in range(1018):
    try:
        column_drop_data['playtime'][i] = float(column_drop_data['playtime'][i])
    except:
        print(f'{i}번째')
In data frame the 'playtime' is str data so, using float method to change data type. 

preprocess_data = column_drop_data
preprocess_data.to_csv('preprocess_Eternal_Return.csv')
#Save data to CSV file.
```