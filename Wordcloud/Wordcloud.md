# Wordcloud
Wordcloud is a package in python. 
We can show frequency of specific words. 
If you want to use Wordcloud not in english you should import font of your language.
Basic import code is like this.
```from wordcloud import WordCloud, STOPWORDS```
In this code, STOPWORDS means remove meanless word token like be verb, subjects(I, you, ...). 
These data will disturb your research when you organize data.
However, STOPWORDS in this package is only for English, so if you want to use it in other languages, you should import different packages for your country.