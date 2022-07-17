# Book Recommendation system for university students 

Hello, this is Nelchois.
This project is group project that my learning class.
---
## Probelm recognition
Now days, reading volume is decreasing in South Korea. 
Because of that, reading comprehension and literacy are pretty decreased. 
So, our team taught that we should care the reading volume at least university students that same as our ages.
---
## Basic obstruction
We taught that there are 3 obstructions of reading volume.
1. Book cost, recently most of the books are more than $150 in each. It's pretty expensive for students.
2. Information about the book, if someone makes up his mind about reading, however, it's hard to know about what books exist, at most bestseller that is too narrow.
3. Interest, even if we pass the obstruction 2 and get some books, there is no guarantee that it will suit our interest. 

## Solving concept
We're using library data to solve book cost, because that books are all free for our target users.
To solve obstruction 2 and 3, we take user's major data that user input directly.If we recommend book automatically, then user don't care about book information and interest. 
First, we collected books data(title, summary, genre) from library pages in my university(we did only one website, because, most of librarys in university have similar books group and recommendation list).
Second, we collected major data(noun keywords) from curriculum subject in each major.
Third, we using Okt(Korean morphs analyzer), N-gram, Sentence-transformer that get embedding value of keywords in data(book's summary, major data). 
Last, visualize system by PYQT5, becasuse it's easy tool that use and make UI.
I designed UI that two space for input and press button for activate.

# Realization
[Crawling](https://github.com/Nelchois/TIL/blob/master/Semi_group_project/Semi_project01.md) We using selenium, because, it's intuitive. We accessed to library page by webdriver and get url of books. From url, we took title, writer, number of books borrowed. Then, code make DataFrame by pandas.

[Preprocessing](https://github.com/Nelchois/TIL/blob/master/Semi_group_project/Semi_project02.md) After crawling, we dropped overlapped words and unnecessary words containing Special Characters. We used Okt model that analyzer about Korean morphs.

[N_gram, Embedding](https://github.com/Nelchois/TIL/blob/master/Semi_group_project/Semi_project02.md) We splited summary data about noun and dropped other data, then using tri_gram to predict the corpus. After that we got embedding scores about words in corpus and major data(noun). We used Key_Bert model('sentence-transformers/xlm-r-100langs-bert-base-nli-stsb-mean-tokens') that pre_trained for keywords. 

[Cos_similarity](https://github.com/Nelchois/TIL/blob/master/Semi_group_project/Semi_project03.md) From embedding value in dataframe, we compared with major value and got similarity score of books, then make books list(upper 10) that have high score.

[Visualize](https://github.com/Nelchois/TIL/blob/master/Semi_group_project/Semi_project03.md) Making code to fucntion and connect them with UI that made by Pyqt5. We let two text box for user's data(major and grade) also there's button for start process, after that recommended books are appear on the hidden text box screen. I designed frameless and hidden style because it's better for me.  