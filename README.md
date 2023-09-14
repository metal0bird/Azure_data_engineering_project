# Azure data engineering project

An End to End Data Engineering project using Azure tools:

It's all about using the smart tools in Azure to turn ordinary raw data into useful insights that we can actually use. This project shows just how powerful Azure Data Factory, Data Lake Gen 2, Synapse Analytics, Azure Databricks, and Power BI can be when it comes to working with data.I embarked on a profound analysis of Netflix's extensive array of shows and movies, employing Exploratory Data Analysis (EDA) techniques. The culmination of this endeavor was a finely-tuned recommendation system, meticulously designed to anticipate user preferences. Paired with dynamic data visualizations, this project epitomizes the seamless fusion of data.

## Insights 

### Visits with recommendation clicks

<img width="903" alt="Screenshot 2023-09-14 at 9 31 30 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/442b0086-658d-4ef0-8d95-740055bee95e">

### Movies vs TV shows


<img width="616" alt="Screenshot 2023-09-14 at 9 31 37 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/c157a749-bca8-4b62-a951-ecd6f4156003">

It is evident that there are more Movies on Netflix than TV shows.

### Best time to release content

If a producer wants to release some content, which month must he do so?( Month when least amount of content is added)

```python
netflix_date = netflix_shows[['date_added']].dropna()
netflix_date['year'] = netflix_date['date_added'].apply(lambda x : x.split(', ')[-1])
netflix_date['month'] = netflix_date['date_added'].apply(lambda x : x.lstrip().split(' ')[0])

month_order = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'][::-1]
df = netflix_date.groupby('year')['month'].value_counts().unstack().fillna(0)[month_order].T
plt.figure(figsize=(10, 7), dpi=200)
plt.pcolor(df, cmap='afmhot_r', edgecolors='white', linewidths=2) # heatmap
plt.xticks(np.arange(0.5, len(df.columns), 1), df.columns, fontsize=7, fontfamily='serif')
plt.yticks(np.arange(0.5, len(df.index), 1), df.index, fontsize=7, fontfamily='serif')

plt.title('Netflix Contents Update', fontsize=12, fontfamily='calibri', fontweight='bold', position=(0.20, 1.0+0.02))
cbar = plt.colorbar()

cbar.ax.tick_params(labelsize=8) 
cbar.ax.minorticks_on()
plt.show()
```

<img width="914" alt="Screenshot 2023-09-14 at 9 36 12 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/35b2b4fa-e4bd-45af-8b3b-5959b5e4776c">

If the latest year 2019 is considered, January and December were the months when comparatively much less content was released.Therefore, these months may be a good choice for the success of a new release!

### Movie ratings analysis

<img width="919" alt="Screenshot 2023-09-14 at 9 36 20 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/03943a97-e5f2-4e9d-94ff-c63c7191cf06">

The largest count of movies are made with the 'TV-MA' rating."TV-MA" is a rating assigned by the TV Parental Guidelines to a television program that was designed for mature audiences only.

Second largest is the 'TV-14' stands for content that may be inappropriate for children younger than 14 years of age.

Third largest is the very popular 'R' rating.An R-rated film is a film that has been assessed as having material which may be unsuitable for children under the age of 17 by the Motion Picture Association of America; the MPAA writes "Under 17 requires accompanying parent or adult guardian".

### Year wise analysis

<img width="928" alt="Screenshot 2023-09-14 at 9 36 27 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/703dd11f-3641-418e-aa53-8689a2ac680c">

So, 2017 was the year when most of the movies were released.

### WordCloud for Genres

```python
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
from PIL import Image

text = list(set(gen))
plt.rcParams['figure.figsize'] = (13, 13)

#assigning shape to the word cloud
mask = np.array(Image.open('/Users/aman/Downloads/star.png'))
wordcloud = WordCloud(max_words=1000000,background_color="white",mask=mask).generate(str(text))

plt.imshow(wordcloud,interpolation="bilinear")
plt.axis("off")
plt.show()
```

<img width="663" alt="Screenshot 2023-09-14 at 9 36 40 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/06867268-991d-4c2c-83f9-1f84c4eb2f74">

<img width="725" alt="Screenshot 2023-09-14 at 9 37 05 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/36e990c7-faf0-43a0-8645-7e48c8b51759">

### Most content creating countries

<img width="987" alt="Screenshot 2023-09-14 at 10 37 12 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/ef9afbc2-1db3-442d-9f60-70e6a6a2c1b2">

Naturally, United States has the most content that is created on netflix in the tv series category.

### Recommendation System

<img width="303" alt="Screenshot 2023-09-14 at 9 37 12 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/6a8fb2c8-c6f1-4214-a3a1-d4e88396f1f2">

The TF-IDF(Term Frequency-Inverse Document Frequency (TF-IDF) ) score is the frequency of a word occurring in a document, down-weighted by the number of documents in which it occurs. This is done to reduce the importance of words that occur frequently in plot overviews and therefore, their significance in computing the final similarity score.

There are about 16151 words described for the 6234 movies in this dataset.

```python
def get_recommendations(title, cosine_sim=cosine_sim):
    idx = indices[title]

    # Get the pairwsie similarity scores of all movies with that movie
    sim_scores = list(enumerate(cosine_sim[idx]))

    # Sort the movies based on the similarity scores
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)

    # Get the scores of the 10 most similar movies
    sim_scores = sim_scores[1:11]

    # Get the movie indices
    movie_indices = [i[0] for i in sim_scores]

    # Return the top 10 most similar movies
    return netflix_overall['title'].iloc[movie_indices]
```

<img width="446" alt="Screenshot 2023-09-14 at 9 37 21 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/9698f496-5bea-4de0-85e9-e29231bd7ca1">

<img width="984" alt="Screenshot 2023-09-14 at 9 37 44 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/ca92558e-e23f-4e94-b10c-0a85a1ace92d">

Here, The Cosine similarity score is used since it is independent of magnitude and is relatively easy and fast to calculate.

Content based filtering on the following factors:

- Title
- Cast
- Director
- Listed in
- Plot

```python
def create_soup(x):
    return x['title']+ ' ' + x['director'] + ' ' + x['cast'] + ' ' +x['listed_in']+' '+ x['description']

def get_recommendations_new(title, cosine_sim=cosine_sim):
    title=title.replace(' ','').lower()
    idx = indices[title]

    # Get the pairwsie similarity scores of all movies with that movie
    sim_scores = list(enumerate(cosine_sim[idx]))

    # Sort the movies based on the similarity scores
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)

    # Get the scores of the 10 most similar movies
    sim_scores = sim_scores[1:11]

    # Get the movie indices
    movie_indices = [i[0] for i in sim_scores]

    # Return the top 10 most similar movies
    return netflix_overall['title'].iloc[movie_indices]
```

<img width="968" alt="Screenshot 2023-09-14 at 9 38 00 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/53467007-3f0d-482a-bf81-0404434079ab">

<img width="966" alt="Screenshot 2023-09-14 at 9 38 12 PM" src="https://github.com/metal0bird/Azure_data_engineering_project/assets/71923741/edcb8503-7f3d-456e-9304-fdf74d968569">

Visit - [My blog](https://dev.to/metal0bird/end-to-end-netflix-data-analytics-project-using-microsoft-azure-tools-45bd)

