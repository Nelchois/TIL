# Machine Learning
Here's example code based on machine learning.

```
#EX1 Load the boston fileset and make datasets based on boston file, after that make visualized graph.
import pandas as pd
t = load_boston()
df = pd.DataFrame(t.data, columns=t.feature_names)
x = t.data
y = t.target
from sklearn.linear_model import LinearRegression
m = LinearRegression()
m.fit(x, y)
out_d = m.predict(t.data)
plt.scatter(y, out_d)
plt.show()
t1 = load_iris()
from sklearn.svm import SVC 
f = [2,3]
X = t1.data[:,f]
Y = t1.target
m = SVC(kernel = 'linear', random_state= 0)
m.fit(X,Y)
X_min = X[:,0].min() -1
X_max = X[:,0].max() +1
Y_min = X[:,1].min() -1
Y_max = X[:,1].max() +1
XX, YY = np.meshgrid(np.linspace(X_min, X_max, 1000), np.linspace(Y_min, Y_max, 1000))
ZZ = m.predict(np.c_[XX.ravel(), YY.ravel()]).reshape(XX.shape)
plt.contourf(XX, YY, ZZ)
plt.scatter(X[Y == 0,0], X[Y == 0,1], s = 20, label = t1.target_names[0])
plt.scatter(X[Y == 1,0], X[Y == 1,1], s = 20, label = t1.target_names[1])
plt.scatter(X[Y == 2,0], X[Y == 2,1], s = 20, label = t1.target_names[2])
```

```
#EX2 Load the data1_all fileset and make datasets, after that get test scores and compare them.
import pandas as pd
import numpy as np
csv_data = pd.read_csv('data1_all.csv')
csv_data
data_X = csv_data[['Weight', 'Length', 'Diagonal', 'Height', 'Width']].to_numpy()
data_Y = csv_data[['Name']].to_numpy()
from sklearn.preprocessing import StandardScaler
ss = StandardScaler().fit(data_X)
s_data_x = ss.transform(data_X)
from sklearn.model_selection import train_test_split
train_x, test_x, train_y, test_y = train_test_split(s_data_x, data_Y, random_state= 42)
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier(n_neighbors= 3)
kn.fit(train_x, train_y)
kn.score(train_x, train_y), kn.score(test_x, test_y)
kn.predict(train_x[:4])
```

```
import pandas as pd
dataf = pd.read_csv('day5_data1.csv')
dataf
dataf.pop('who')
dataf.pop('Country')
dataf.pop('Years on Internet')
dataf.dtypes
check_c = ['Gender', 'Household Income', 'Sexual Preference', 'Education Attainment', 'Major Occupation', 'Marital Status']
for i in check_c:
    dataf[i] = dataf[i].astype('category')
dataf.dtypes
dataf_one_hot = pd.get_dummies(dataf)
dataf_one_hot.loc[pd.isnull(dataf_one_hot['Age']), 'Age'] = dataf_one_hot['Age'].mean()
```

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
X = np.array(
    [8.4, 13.7, 15.0, 16.2, 17.4, 18.0, 18.7, 19.0, 19.6, 20.0, 
     21.0, 21.0, 21.0, 21.3, 22.0, 22.0, 22.0, 22.0, 22.0, 22.5, 
     22.5, 22.7, 23.0, 23.5, 24.0, 24.0, 24.6, 25.0, 25.6, 26.5, 
     27.3, 27.5, 27.5, 27.5, 28.0, 28.7, 30.0, 32.8, 34.5, 35.0, 
     36.5, 36.0, 37.0, 37.0, 39.0, 39.0, 39.0, 40.0, 40.0, 40.0, 
     40.0, 42.0, 43.0, 43.0, 43.5, 44.0]
     )
Y = np.array(
    [5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0, 85.0, 85.0, 
     110.0, 115.0, 125.0, 130.0, 120.0, 120.0, 130.0, 135.0, 110.0, 
     130.0, 150.0, 145.0, 150.0, 170.0, 225.0, 145.0, 188.0, 180.0, 
     197.0, 218.0, 300.0, 260.0, 265.0, 250.0, 250.0, 300.0, 320.0, 
     514.0, 556.0, 840.0, 685.0, 700.0, 700.0, 690.0, 900.0, 650.0, 
     820.0, 850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0, 1100.0, 
     1000.0, 1000.0]
     )

from sklearn.model_selection import train_test_split
test_x, target_x, test_y, target_y = train_test_split(X, Y, random_state = 43) 
print(test_x.shape, target_x.shape)
n_test_x = test_x.reshape(-1,1)
n_target_x = target_x.reshape(-1,1)

from sklearn.neighbors import KNeighborsRegressor
knr = KNeighborsRegressor()
knr.n_neighbors = 3
knr.fit(n_test_x, test_y)
knr.score(n_target_x, target_y)
knr.score(n_test_x, test_y)

from sklearn.metrics import mean_absolute_error
end_target_y = knr.predict(n_target_x)
mae = mean_absolute_error(target_y, end_target_y)
print(mae)

knr2 = KNeighborsRegressor()
array_x = np.arange(45).reshape(-1,1)
for n in [1, 5, 10]:
    knr2.n_neighbors = n
    knr2.fit(n_test_x, test_y)
    p_data = knr2.predict(array_x)
    plt.scatter(n_test_x, test_y)
    plt.plot(array_x, p_data)
    plt.title(f'n_plt{n}')
    plt.show()
```