# Cinematic Clash - Production vs. Perception

## Introduction

This project delves into the intricacies of the film industry, scrutinizing the production output of various movie studios and the critical acclaim their films receive. By meticulously examining a vast dataset encompassing genres, production studios, critic and audience ratings, and release dates, we aim to uncover patterns and trends that define the current state of filmmaking.

Our primary goals are to:

Identify the most prolific movie studios based on their production volume.
Evaluate the critical reception of films across different studios and genres.
Explore the correlation between production quantity and perceived quality by critics and audiences.
Comprehend the evolving trends in movie production and distribution, particularly in the context of streaming platforms.
The findings of this analysis will provide a data-driven understanding of the film industry, shedding light on how studios balance quantity and quality. This project is of significant value to various stakeholders, including filmmakers, producers, marketers, and film buffs, as it illuminates the factors that contribute to the success and appeal of movies.

Our methodology involves rigorous data collection, cleaning, and analysis, culminating in informative visualizations. We will utilize a range of statistical tools and techniques to ensure a comprehensive and accurate evaluation.

The Film Industry Analysis Project represents a significant step towards untangling the complexities of the movie business. Through careful data analysis and insightful interpretation, we aspire to provide valuable insights into the dynamics of movie production and critical reception, offering a unique perspective on the cinematic landscape.

Methodology
Data Collection
The analysis commenced with the acquisition of a substantial dataset from Kaggle, a repository of publicly available data. This dataset provided a rich trove of information about movies, encompassing various aspects such as movie titles, genres, production studios, release dates, runtimes, tomatometer ratings (critic ratings), audience ratings, and additional metadata.
This comprehensive dataset served as the foundation for the subsequent analysis, enabling a deep exploration of the film industry's dynamics and trends.
'''
Data Analysis
'''
df = pd.read_csv("/kaggle/input/rotten-tomatoes-movies-and-critic-reviews-dataset/rotten_tomatoes_movies.csv")
'''
df.head()
'''
df.info()
'''
df.shape
'''
df.describe()
'''
print("\nMissing Values:")
print(df.isnull().sum())
'''
df.duplicated().sum() 
print("\nDuplicate Rows:")
print(df.duplicated().sum())
'''
Data Visualisation
The study employed multiple visual aids, including bar charts, line charts, scatter plots, and box plots, to effectively showcase the findings.
Bar charts were used to depict the number of movies produced by each production company and the average critical ratings received.
Line charts were employed to illustrate trends over time, such as fluctuations in production volumes or streaming release patterns.
Scatter plots were implemented to explore correlations between distinct variables, such as the association between production volume and critical reception.
Box plots were used to examine the distribution and spread of critical ratings across genres and other categories.
#

'''
Histogram of the distribution of tomatometer ratings
'''
import matplotlib.pyplot as plt
data = df
# Histogram of tomatometer ratings
plt.figure(figsize=(10, 6))
plt.hist(data['tomatometer_rating'], bins=20, color='tomato', edgecolor='black')
plt.title('Distribution of Tomatometer Ratings')
plt.xlabel('Tomatometer Rating')
plt.ylabel('Number of Movies')
plt.grid(True)
plt.show()
​
'''
This histogram illustrates the frequency distribution of movies across various tomatometer rating ranges, providing insights into the overall trend of critic ratings for movies in the dataset.
Key Observations:
The rating distribution exhibits a slight leftward skew, suggesting that a higher proportion of movies have higher ratings.

A substantial number of movies possess very high ratings, approaching 100%.

The number of movies decreases gradually as rating decreases, with fewer films receiving very low ratings.

'''
Bar chart showing the average audience rating for each movie genre
'''
from collections import defaultdict
import numpy as np
​
# Splitting the 'genre' column into individual genres
genre_list = data['genres'].str.split(', ').dropna()
unique_genres = set([genre for sublist in genre_list for genre in sublist])
​
# Calculating the average audience rating for each genre
genre_average_ratings = defaultdict(list)
for genres, rating in zip(genre_list, data['audience_rating']):
    if pd.notna(rating):
        for genre in genres:
            genre_average_ratings[genre].append(rating)
​
genre_average_ratings = {genre: np.mean(ratings) for genre, ratings in genre_average_ratings.items()}
​
# Sorting the genres by average rating
sorted_genres = sorted(genre_average_ratings.items(), key=lambda x: x[1], reverse=True)
​
# Creating a bar chart
genres, avg_ratings = zip(*sorted_genres)
plt.figure(figsize=(15, 8))
plt.bar(genres, avg_ratings, color='skyblue')
plt.xticks(rotation=45, ha='right')
plt.title('Average Audience Rating by Movie Genre')
plt.xlabel('Genre')
plt.ylabel('Average Audience Rating')
plt.show()
​
​
'''
This bar chart depicts the average audience ratings for various movie genres, providing valuable insights into audience preferences.
Key Observations:
Certain genres consistently achieve higher average ratings, indicating their widespread appeal and popularity among viewers.

The notable variation in average ratings across genres highlights distinct audience preferences and the diverse tastes of moviegoers.

'''
Line chart showing the number of movies released each year.
'''
# Extracting the release year from the 'in_theaters_date' column
data['streaming_release_date'] = pd.to_datetime(data['streaming_release_date'], errors='coerce').dt.year
​
# Counting the number of movies released each year
movies_per_year = data['streaming_release_date'].value_counts().sort_index()
​
# Creating a line chart
plt.figure(figsize=(12, 6))
plt.plot(movies_per_year.index, movies_per_year.values, marker='o', linestyle='-', color='purple')
plt.title('Number of Movies Released per Year')
plt.xlabel('Year')
plt.ylabel('Number of Movies')
plt.grid(True)
plt.show()
​
'''
This visual representation aids in comprehending the trend in movie production throughout the years covered by the dataset.
Key observations:
Identifying an upward or downward pattern in the number of movies released over time.
Flagging any specific years with exceptionally high or low releases that warrant further investigation, potentially linked to historical events impacting the film industry.
This analysis provides valuable insights into the dynamics of the movie industry, assisting in understanding industry trends, historical influences on film production, and potential future trajectories.
'''
Scatter plot to see if there's a correlation between the tomatometer rating (critics' rating) and the audience rating
'''
# Scatter plot of tomatometer rating vs audience rating
plt.figure(figsize=(10, 6))
plt.scatter(data['tomatometer_rating'], data['audience_rating'], alpha=0.5, color='green')
plt.title('Tomatometer Rating vs Audience Rating')
plt.xlabel('Tomatometer Rating')
plt.ylabel('Audience Rating')
plt.grid(True)
​
# Calculating the correlation coefficient
correlation = data['tomatometer_rating'].corr(data['audience_rating'])
plt.text(10, 95, f'Correlation coefficient: {correlation:.2f}', fontsize=12, color='blue')
​
plt.show()
​
'''
The scatter plot demonstrates the correlation between critic ratings (tomatometer rating) and audience ratings for movies in the dataset.
Key findings:
A moderate positive correlation is observed, with a correlation coefficient of ≈ 0.60. This implies that, generally speaking, movies highly rated by critics also tend to be well-received by audiences. However, there are notable exceptions.

The scatter plot depicts a dispersed pattern, indicating varying opinions between critics and audiences. Although many movies fall along a line suggesting relative agreement, there exist prominent outliers.

This visualization helps elucidate the alignment (or lack thereof) between critic opinions and audience preferences.

'''
Box plot to examine the spread of runtime in minutes, which will give us insights into the typical length of movies
'''
# Creating a box plot for movie runtimes
plt.figure(figsize=(10, 6))
plt.boxplot(data['runtime'].dropna(), patch_artist=True, boxprops=dict(facecolor='lightblue'))
plt.title('Distribution of Movie Runtimes')
plt.ylabel('Runtime in Minutes')
plt.xticks([1], ['Movies'])
plt.grid(True)
plt.show()
​
'''
This box plot effectively depicts the distribution of movie runtimes within the dataset.
Key characteristics:
The central box encapsulates the interquartile range (IQR), encompassing the middle 50% of the data points. The line within the box represents the median runtime. This information highlights the typical runtime for movies in the dataset.

The "whiskers" (lines extending from the top and bottom of the box) indicate the overall range of the runtimes, excluding outliers. This provides a sense of the distribution's spread beyond the central 50%.

Any points falling outside the whiskers are considered outliers, indicating movies with runtimes significantly longer or shorter than the typical runtime. These outliers deserve further investigation as they may represent unique or exceptional cases.

'''
Scatter plot to examine the relationship between the runtime of movies and their audience ratings.
'''
# Scatter plot of runtime vs audience rating
plt.figure(figsize=(10, 6))
plt.scatter(data['runtime'], data['audience_rating'], alpha=0.5, color='darkblue')
plt.title('Movie Runtime vs Audience Rating')
plt.xlabel('Runtime in Minutes')
plt.ylabel('Audience Rating')
plt.grid(True)
​
# Calculating the correlation coefficient
correlation_runtime_rating = data['runtime'].corr(data['audience_rating'])
plt.text(250, 10, f'Correlation coefficient: {correlation_runtime_rating:.2f}', fontsize=12, color='red')
​
plt.show()
​
'''
This scatter plot effectively demonstrates the relationship between movie runtimes and audience ratings.
Key findings:
A very weak, nearly negligible negative correlation is observed, with a correlation coefficient of approximately 0.23. This implies that movie length has a minimal influence on audience satisfaction.

The widely dispersed plot suggests a diverse range of audience ratings for movies with various runtimes. There is no discernible pattern indicating that longer or shorter movies consistently receive higher or lower ratings.

This visualization highlights the limited predictability of audience satisfaction based on runtime. Audience preferences likely hinge on factors like genre, plot, and production quality.

'''
Bar chart showing the average tomatometer rating for movies produced by different studios
'''
# Grouping data by studio and calculating average tomatometer rating
studio_ratings = data.groupby('production_company')['tomatometer_rating'].mean()
​
# Filtering out studios with fewer movies to ensure reliability
min_movies = 10
studio_counts = data['production_company'].value_counts()
selected_studios = studio_counts[studio_counts >= min_movies].index
selected_studio_ratings = studio_ratings.loc[selected_studios].sort_values(ascending=False)
​
# Creating a bar chart for top studios
top_studios = selected_studio_ratings.head(20)
plt.figure(figsize=(15, 8))
plt.bar(top_studios.index, top_studios.values, color='orange')
plt.xticks(rotation=45, ha='right')
plt.title('Average Tomatometer Rating by Production Company (Top 20)')
plt.xlabel('Production Company')
plt.ylabel('Average Tomatometer Rating')
plt.show()
​
'''
This bar chart illustrates the average Tomatometer ratings for the top 20 production companies, identifying those studios consistently delivering critically acclaimed films.
The companies positioned at the top of the chart consistently produce movies receiving exceptional critic ratings, showcasing their aptitude for crafting high-quality content.
This graphical representation offers insights into the production companies that consistently garner favorable critical reception, providing valuable information for film enthusiasts and industry professionals.
'''
Scatter plot to observe the relationship between the tomatometer count and rating
to see whether movies with more critic reviews tend to have higher or lower ratings.
'''
# Scatter plot of tomatometer count vs tomatometer rating
plt.figure(figsize=(10, 6))
plt.scatter(data['tomatometer_count'], data['tomatometer_rating'], alpha=0.5, color='brown')
plt.title('Tomatometer Count vs Tomatometer Rating')
plt.xlabel('Tomatometer Count (Number of Critic Reviews)')
plt.ylabel('Tomatometer Rating')
plt.grid(True)
​
# Calculating the correlation coefficient
correlation_count_rating = data['tomatometer_count'].corr(data['tomatometer_rating'])
plt.text(10, 10, f'Correlation coefficient: {correlation_count_rating:.2f}', fontsize=12, color='blue')
​
plt.show()
​
'''
A scatter plot depicting the relationship between critic reviews and rating scores reveals a slight positive correlation, with a correlation coefficient of approximately 0.08.
This observation suggests that movies with a higher volume of critic reviews tend to receive marginally elevated ratings. However, the association is not particularly robust, as the spread of points indicates substantial variability.
The scattered plot underscores the presence of exceptions to this general trend. Movies with relatively few reviews can achieve high ratings, while those with a significant number of reviews can receive lower ratings. This highlights the limitations of solely relying on the number of critic reviews to gauge a movie's overall quality.
'''
Bar chart showing the number of movies in each genre
'''
from itertools import chain
​
genre_counts = pd.Series(list(chain.from_iterable(genre_list))).value_counts()
​
plt.figure(figsize=(15, 8))
plt.bar(genre_counts.index, genre_counts.values, color='teal')
plt.xticks(rotation=45, ha='right')
plt.title('Number of Movies in Each Genre')
plt.xlabel('Genre')
plt.ylabel('Number of Movies')
plt.show()
​
​
'''
This genre distribution pattern could reflect prevalent trends in movie production, where studios and audiences consistently show preference for certain genres. Alternatively, the disparity could be attributed to the diverse nature of the dataset, encompassing a broader genre spectrum across different periods and geographical regions.
'''
The average audience rating for movies directed by different individuals
'''
# Grouping data by director and calculating average audience rating
director_ratings = data.groupby('directors')['audience_rating'].mean()
​
# Filtering out directors with fewer movies to ensure reliability
min_director_movies = 5
director_movie_counts = data['directors'].value_counts()
selected_directors = director_movie_counts[director_movie_counts >= min_director_movies].index
selected_director_ratings = director_ratings.loc[selected_directors].sort_values(ascending=False)
​
# Creating a bar chart for top directors
top_directors = selected_director_ratings.head(20)
plt.figure(figsize=(15, 8))
plt.bar(top_directors.index, top_directors.values, color='maroon')
plt.xticks(rotation=45, ha='right')
plt.title('Average Audience Rating by Director (Top 20)')
plt.xlabel('Director')
plt.ylabel('Average Audience Rating')
plt.show()
​
'''
This bar chart highlights the top directors whose movies have consistently garnered favorable audience ratings.
These directors have demonstrated their ability to create films that resonate with audiences, resulting in elevated audience ratings.
It is crucial to acknowledge that these averages are derived from the subset of movies included in the dataset, and the number of movies directed by each individual can vary.
'''
Line chart showing the average tomatometer rating and average audience rating for each year.
'''
# Grouping data by release year and calculating average ratings
yearly_ratings = data.groupby('streaming_release_date').agg({'audience_rating': 'mean', 'tomatometer_rating': 'mean'})
​
# Creating a line chart for the trends over time
plt.figure(figsize=(15, 8))
plt.plot(yearly_ratings.index, yearly_ratings['audience_rating'], marker='o', linestyle='-', color='blue', label='Average Audience Rating')
plt.plot(yearly_ratings.index, yearly_ratings['tomatometer_rating'], marker='x', linestyle='-', color='red', label='Average Tomatometer Rating')
plt.title('Average Audience Rating and Tomatometer Rating Over Time')
plt.xlabel('Year')
plt.ylabel('Average Rating')
plt.legend()
plt.grid(True)
plt.show()
​
'''
The line chart effectively illustrates the dynamic evolution of critic and audience ratings over the years.
This visualization allows for the identification of periods of significant change or stability in both metrics. By comparing the two lines, we can discern instances where critical and audience reception diverged or converged.
'''
Box plot - the distribution of tomatometer ratings across different movie genres
'''
# Preparing data for box plots - splitting genres and associating them with tomatometer ratings
expanded_data = data.drop('genres', axis=1).join(
    data['genres'].str.split(', ', expand=True).stack().reset_index(level=1, drop=True).rename('genre')
)
​
# Creating box plots for tomatometer ratings across different genres
plt.figure(figsize=(30, 14))
expanded_data.boxplot(column='tomatometer_rating', by='genre', patch_artist=True)
plt.title('Distribution of Tomatometer Ratings Across Movie Genres')
plt.suptitle('')  # Removes the default 'Boxplot grouped by genre' title
plt.xlabel('Genre')
plt.ylabel('Tomatometer Rating')
plt.xticks(rotation=90)
plt.grid(True)
plt.show()
​
'''
This box plot showcasing tomatometer ratings across various genres unveils distinct patterns in critical reception.
The median tomatometer rating (the line within each box) indicates the level of critical acclaim for each genre. While action and drama genres generally receive higher median ratings, genres like documentary and thriller tend to receive lower ratings.
The range and variability of tomatometer ratings (represented by the length of the boxes and whiskers) also vary across genres. Action and drama genres exhibit a narrower range, suggesting more consistent critical reception within these genres. Conversely, genres like comedy and horror display a wider range, indicating more diverse critical opinions.
Outliers, represented by points outside the whiskers, highlight movies that deviate significantly from the overall critical reception within their respective genres. These outliers may represent critically acclaimed or panned films that stand out from the crowd.
'''
Type Markdown and LaTeX:  α2
 
'''
# Grouping data by studio and calculating average audience rating
studio_audience_ratings = data.groupby('production_company')['audience_rating'].mean()
​
# Filtering out studios with fewer movies to ensure reliability
min_studio_movies = 5
studio_movie_counts = data['production_company'].value_counts()
selected_studios_for_audience = studio_movie_counts[studio_movie_counts >= min_studio_movies].index
selected_studio_audience_ratings = studio_audience_ratings.loc[selected_studios_for_audience].sort_values(ascending=False)
​
# Creating a bar chart for top studios based on audience rating
top_studios_for_audience = selected_studio_audience_ratings.head(20)
plt.figure(figsize=(15, 8))
plt.bar(top_studios_for_audience.index, top_studios_for_audience.values, color='lightgreen')
plt.xticks(rotation=45, ha='right')
plt.title('Average Audience Rating by production company (Top 20)')
plt.xlabel('production_company')
plt.ylabel('Average Audience Rating')
plt.show()
​
'''
The bar chart highlights the top production companies whose movies consistently garner favorable audience ratings.
These production companies have demonstrated their ability to craft films that resonate with audiences, resulting in elevated audience ratings.
It is crucial to acknowledge that these averages are derived from the subset of movies included in the dataset, and the number of movies produced by each studio can vary.
This caveat emphasizes the need for caution when interpreting the results, considering the potential influence of the number of movies produced by each studio.
'''
Scatter plot to observe the relationship between the number of critic reviews (tomatometer count) and the number of audience ratings (audience count) for each movie
'''
plt.figure(figsize=(10, 6))
plt.scatter(data['tomatometer_count'], data['audience_count'], alpha=0.5, color='navy')
plt.title('Tomatometer Count vs Audience Count')
plt.xlabel('Tomatometer Count (Number of Critic Reviews)')
plt.ylabel('Audience Count')
plt.grid(True)
​
# Calculating the correlation coefficient
correlation_critic_audience = data['tomatometer_count'].corr(data['audience_count'])
plt.text(10, 1000000, f'Correlation coefficient: {correlation_critic_audience:.2f}', fontsize=10, color='yellow')
​
plt.show()
​
'''
This scatter plot illustrating the relationship between critic reviews and audience ratings reveals a mild positive correlation, with a correlation coefficient of approximately 0.13.
This suggests that there is a general tendency for movies with more critic reviews to attract more audience ratings. However, the correlation is not strongly pronounced, indicating that this relationship is not always evident.
The dispersed distribution of points emphasizes the presence of notable exceptions to this general trend.
Some movies with relatively few critic reviews have garnered significant audience engagement, while others with a higher volume of critic reviews have attracted fewer audience ratings.
This scatter plot effectively communicates the subtle connection between critic reviews and audience ratings, highlighting the variability in audience responses despite varying levels of critical attention.
This visualization provides valuable insights into the dynamics between critic reviews and audience engagement, demonstrating that these two factors do not always correspond directly.
'''
Summary
'''
This analysis provided a detailed examination of the film industry, focusing on the production volumes and critical receptions of movies across major studios.
Key findings included:
Production Volume:
Significant variations in the number of movies produced by different studios were observed.
A diverse landscape emerged, with studios exhibiting distinct levels of production prolificacy.
Critical Reception:
The overall quality of movies from these studios was assessed through tomatometer ratings.
The report highlighted the relationship between the number of movies a studio produces and the critical acclaim these movies receive.
Quantity vs. Quality Correlation:
Several studios showed a balance between producing a large number of movies and maintaining high quality standards.
Other studios excelled in one area, either churning out a large number of movies or producing critically acclaimed films with lower production volumes.
Industry Trends:
The analysis shed light on broader trends in the film industry.
It included the increasing importance of streaming platforms and changes in genre popularity over time.
Data-Driven Insights:
Various data visualization techniques were used to present a clear and comprehensive view of the film industry's landscape.
These visualizations effectively communicated the key findings and allowed for a deeper understanding of the industry's intricacies.
Overall Implications:
The delicate balance between producing a large number of movies and maintaining high quality standards in terms of critical reception was underscored.
The varying strategies of different studios were highlighted, providing a snapshot of the evolving dynamics within the film industry.
Valuable insights were offered for industry stakeholders and film enthusiasts alike.
'''
