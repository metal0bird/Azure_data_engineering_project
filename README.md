# Azure data engineering project

An End to End Data Engineering project using Azure tools:

It's all about using the smart tools in Azure to turn ordinary raw data into useful insights that we can actually use. This project shows just how powerful Azure Data Factory, Data Lake Gen 2, Synapse Analytics, Azure Databricks, and Power BI can be when it comes to working with data.I embarked on a profound analysis of Netflix's extensive array of shows and movies, employing Exploratory Data Analysis (EDA) techniques. The culmination of this endeavor was a finely-tuned recommendation system, meticulously designed to anticipate user preferences. Paired with dynamic data visualizations, this project epitomizes the seamless fusion of data.

##Insights 

###Visits with recommendation clicks
```python
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

###Movies vs TV shows

It is evident that there are more Movies on Netflix than TV shows.

###Best time to release content
If a producer wants to release some content, which month must he do so?( Month when least amount of content is added)

If the latest year 2019 is considered, January and December were the months when comparatively much less content was released.Therefore, these months may be a good choice for the success of a new release!

###Movie ratings analysis

The largest count of movies are made with the 'TV-MA' rating."TV-MA" is a rating assigned by the TV Parental Guidelines to a television program that was designed for mature audiences only.

Second largest is the 'TV-14' stands for content that may be inappropriate for children younger than 14 years of age.

Third largest is the very popular 'R' rating.An R-rated film is a film that has been assessed as having material which may be unsuitable for children under the age of 17 by the Motion Picture Association of America; the MPAA writes "Under 17 requires accompanying parent or adult guardian".

###Year wise analysis

So, 2017 was the year when most of the movies were released.

###WordCloud for Genres


###Genres vs count 
Lollipop plot of Genres vs their count on Netflix


###Most content creating countries

Naturally, United States has the most content that is created on netflix in the tv series category.

###TV shows with largest number of seasons 


Thus, NCIS, Grey's Anatomy and Supernatural are amongst the tv series that have highest number of seasons.

###Recommendation System

The TF-IDF(Term Frequency-Inverse Document Frequency (TF-IDF) ) score is the frequency of a word occurring in a document, down-weighted by the number of documents in which it occurs. This is done to reduce the importance of words that occur frequently in plot overviews and therefore, their significance in computing the final similarity score.

There are about 16151 words described for the 6234 movies in this dataset.

Here, The Cosine similarity score is used since it is independent of magnitude and is relatively easy and fast to calculate.

Content based filtering on the following factors:

-Title
-Cast
-Director
-Listed in
-Plot
Visit - [My blog](https://dev.to/metal0bird/end-to-end-netflix-data-analytics-project-using-microsoft-azure-tools-45bd)

