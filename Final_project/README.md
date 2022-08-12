# SAPAGO PROJECT
Hello, this is Nelchois. 
Now we started final project that end of our course.

## Problem recognize
Now days, many people do used goods trading. In there, smart device including smart phone is by far the most popular product. There are various reasons, however, the biggest reason is price of new product. The price of smartphone is ridiculously over $800. In depression, it's too much expenditure. But, there are some discomfots for trading. So we make price suggestion program with their status. If user input their product's name and condition in sentence, then program analyze and classify that sentence. After that it use that data and print the average price.

## Solution concept
First, we get product's name and status from users, then trained model classify that status and make label 0 to 5. 0 is perfect one that just opened one even not opened products. 4 is worst one that broken products and 5 is Error message that can't classify. 
Input message form 'Product name : status sentence' in one line, this line will split about ':' in code. System analyze status sentence and make the label, after that find same products by product name and status label.  

Second, search data from dataframe by product's name with same label. After remove outlier and make average price about data.
Third, send the answer to chat bot in Kakao openbuilder system, then print the answer to user by chat bot. Kakao openbuilder is free for 50,000 chat, after that 30won about 1 case. However, after september in 2022 their service will be free.

### [Crawling](https://github.com/Nelchois/TIL/blob/master/Final_project/Sapago_crawling.md)
We check three used goods market sites that most famous in south korea. We make 72 products list in smart phone and tablet pc. We take data from web site and remove commercial data or fake products. After that we use our classifier and split data in 5 labels, 0 is perfect one that just open or not opened products. 1 is clean and not scratched one. 2 is clean ,but repaired. 3 is usable products with little scratch. 4 is broken one or malfunctioning products.

### [Classify model](https://github.com/Nelchois/TIL/blob/master/Final_project/BERT_Classifier.md)
 We use pretrained_language model for classifier([LMkor](https://github.com/kiyoungkim1/LMkor)). LMkor was trained by commercial reviews by custommer or text from blog that more suitable to Sapago Project, because other korean language models are trained by news text or political documents that grammar is correct and rigid, however our service need user's input that may be included inscriptions or typos. So we use LMkor which similar text that we use.

 ### [Chat bot](https://github.com/Nelchois/TIL/blob/master/Final_project/application.py)
 We use Kakao i open builder api, so we make the code based on json, because chat bot send the user's input to server by json file, then server replies answer to chat bot by json structure that makes chat bot convert to the specific answer style.
 