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
```