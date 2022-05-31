# Machine learning
Machine learning is technology that train pattern and rule in data to computer, then computer can solve problems based on mathmatical models in trained data.
There is subconcept of machine learning, which is deep learning.
deep learning is similar to machine learning, but deep learning normally use neural netwrok structure.

Process of machine learning.
1. Get training datasets.
2. Input data with preprocessing
3. Machine learning
4. Predict by using test datasets.

## Basic concept of Machine Learning
* algorithms : the method or order to solve the problem.

* machine learning : let machine learn about pattern and make the automatic algorithms.

* big data : so huge data that is hard to collect, save and analyze in existing database.
Machine learning has basic structure that y = f(x). Machine learning is a 
procedure to find that y with use data(x). It will present f(), which we want.

* supervised learning : we give problems(data(x)) and answers(data(y)) to the  machine that make it finds the rule in problem cases.

* unsupervised learning : it's an opposite of supervised learning. we give only training data(data(x)) and machine find the rule in data itself.

* reinforcement learning : computer learned and simulated rule in the world itself like a game rewards or punishment, generation : make something new didn't exist in the world like chatbot or make human face.

* null type data : existed data, however not recorded data. Even though data is existed, but it's impossible to train model without that data. We usually drop these data for preprocessing. 
    1. ```dropna``` if we use this method, code will delete row that include NaN type data. After using this method, we should make new object or use ```inplace = True```, normally we make new object.
    2. ```drop``` we can set option(how). If we give 'all' to option, code will delete row that all data is NaN type data. On the other way, if we give 'any' to option that make same result for ```dropna``` method. 
    - 'axis' option set applicable axis in method, for example, if we use ```dropna``` method with ```axis = 1``` option, then code delete column instead of row. 
    The second option is ```thresh```, if set thresh = n, code will remain row that has data at least n.
    3. ```fill``` we can fill the null type values after using ```drop```,
    at that time user consider mean, mode, median. 
    ```fillna``` method filled all null type values in data, but be careful, 'fill' method change data directly, so you don't have to use 'inplace'.     

* outlier : values which too big or too small that has big difference with other dataset. Usually it's hard to use these data for training. 

### Classification, Regression
We can solve two types of problems in supervised learning.
One is classification.
In classification, it's really hard to classify real data for human, however if we use machine learning we can solve that problem just time and data for training.
There is no reason for wrestle with the calculator.

The other one is regression.
In regression, if we give data(x) then machine use model and predict answer data(y).
regression has simple structure : y = ax + b, yeah it's really simple, however find the answer is tough one, because we must find total model with a & b = two variants only data(x) and y. 