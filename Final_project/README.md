# SAPAGO PROJECT
Hello, this is Nelchois. 
Now we started final project that end of our course.

## Problem recognize
Now days, many people do used goods trading. In there, smart device including smart phone is by far the most popular product. There are various reasons, however, the biggest reason is price of new product. The price of smartphone is ridiculously over $800. In depression, it's too much expenditure. But, there are some discomfots for trading. So we make price suggestion program with their status. If user input their product's name and condition in sentence, then program analyze and classify that sentence. After that it use that data and print the average price.

## Solution concept
First, we get product's name and status, then trained model classify that status and make label 0 to 5. 0 is perfect one that just opened one even not opened products.
Second, search data from dataframe by product's name with same label. After remove outlier and make average price about data.
Third, send the answer to chat bot in Kakao openbuilder system, then print the answer to user by chat bot. Kakao openbuilder is free for 50,000 chat, after that 30won about 1 case.