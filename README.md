# Data Wrangling Project
This is the project assignment from the UT MSIS course INF 385T _Data Wrangling_  with Prof James Howison.
<br>This project allowed me an opportunity to utilize `SQL` and `Python`.
<br>This project is collaborated with Philo Lin.

## Project Description
### Can IMDB movies of origin in the U.S. have an impact on international tourists visiting the U.S.?


It’s no doubt that the US entertainment industry such as Hollywood has a significant global impact. Every year, countless individuals from around the world travel to the United States. We would like to find out if there is any connection between movies and tourism. Can a film genuinely influence the choice of destination for international travelers?  If this holds true, we can demonstrate that blockbuster movies offer an additional advantage beyond their box office revenue. For this project, our primary focus will be on examining the connection between movies shot in the United States and the impact on United States tourism.

### Goals:
- Analyzing the blockbuster movies shot in the United States in the past ten pre-pandemic years (2009-2019), to find out the relationship between the filming industry and the tourism industry in the United States.
- Especially, the impact of the entertainment industry on the willingness of overseas tourists to visit the United States.
- Using the date the blockbuster movies get released to make a connection between the blockbuster movies in the United States and international tourism.

## Process

**Collect data**
- Collect and filter raw datasets from various websites.

| Dataset | Resources Link |
|-----:|---------------|
|imdb_movies.csv|[IMDB dataset](https://www.kaggle.com/datasets/ashpalsingh1525/imdb-movies-dataset/data)|
|tourism_inbound.csv|[Inbound Tourism](https://stats.oecd.org/Index.aspx?DataSetCode=TOURISM_INBOUND)|
|countries.csv|[World Countries](https://stefangabos.github.io/world_countries/)|

- Resave the datasets into CSV format.
<br>

**Create new databases**
- Create new tables with SQL in duckdb

- Insert data from CSV files using Python
<br>

**Query Data**
- Query data from SQL tables using Python
<br>

**Analysis Data**
- Using Matplotlib to generate bar and line plot graph
- Eyeball observes the correlation between movie ratings and tourist number
<br>

**Draw Conclusion**


