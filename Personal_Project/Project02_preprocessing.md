This code is connected to Project01.
```
data.replace({'recommend': 'Not Recommended'}, 0, inplace = True)
data.replace({'recommend': 'Recommended'}, 1, inplace = True)
```
In DataFrame, recommend and not recommend are to complex, so I change recommend to 1 and not recommend to 0.

```
data.to_csv('steam_Eternal_Return_review.csv')

using_data = pd.read_csv('steam_Eternal_Return_review.csv', encoding = 'utf-8')

using_data
```
Save the Data to CSV.

