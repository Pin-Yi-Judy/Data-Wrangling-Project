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
![image](https://github.com/Pin-Yi-Judy/Data-Wrangling-Project/blob/main/Images/Workflow%20Image.jpg)

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
```
%%sql
DROP TABLE IF EXISTS countries;
DROP SEQUENCE IF EXISTS seq_countries_id;
CREATE SEQUENCE seq_countries_id START 1;
CREATE TABLE countries (
    id INT PRIMARY KEY DEFAULT NEXTVAL('seq_countries_id'),
    name TEXT
);
```
- Insert data from CSV files using Python
```
with duckdb.connect('duckdb-file.db') as con:
    
    sql_find_country = """
    SELECT id FROM countries 
    WHERE name = $new_name
    """
    
    sql_insert_country = """
    INSERT INTO countries(name)
    VALUES ($new_name) 
    RETURNING id
    """

    with open('countries.csv') as csvfile:
        myCSVReader = csv.DictReader(csvfile, delimiter=",", quotechar='"')
        # {'id': '4', 'alpha2': 'af', 'alpha3': 'afg', 'name': 'Afghanistan'}
        for row in myCSVReader:
            param_dict = {'new_name': row['name']}
            
            # Check if the country already exists in the database
            con.execute(sql_find_country, param_dict)
            existing_id = con.fetchone()

            if existing_id:
                country_id = existing_id[0]
            else:
                # If the country doesn't exist, insert it into the database
                con.execute(sql_insert_country, param_dict)
                country_id = con.fetchone()[0]
```
**Query & Analysis Data**
- Query data from SQL tables using Python
- Using Matplotlib to generate bar and line plot graph
```
years = []
tourist_numbers = []

with duckdb.connect('duckdb-file.db') as con:
    # Execute query
    sql_query = """
    SELECT SUM(tourist_number), year
    FROM tourist_information
    JOIN movies
        ON tourist_information.year_id = movies.year_id
    JOIN country_names
        ON tourist_information.country_name_id = country_names.id
    JOIN countries
        ON countries.id = country_names.country_id
    JOIN years
        ON tourist_information.year_id = years.id
    WHERE countries.name = 'United States of America'
    AND years.year BETWEEN 2009 AND 2019
    GROUP BY year
    ORDER BY year
    """
    result = con.execute(sql_query)

    # Fetch and print the results
    rows = result.fetchall()
    for row in rows:
        tourist_number, year = row
        years.append(year)
        tourist_numbers.append(tourist_number)

# Create a bar plot
plt.bar(years, tourist_numbers)

# Add labels and title
plt.xlabel('Years')
plt.ylabel('Total Inbound Tourists')
plt.title('Years vs. Total Inbound Tourists')

# Display the plot
plt.show()
```
![image](https://github.com/Pin-Yi-Judy/Data-Wrangling-Project/blob/main/Images/Years%20v.s.%20Total%20Inbound%20Tourists.jpg)
![image](https://github.com/Pin-Yi-Judy/Data-Wrangling-Project/blob/main/Images/Years%20vs%20Average%20Movie%20Ratings.jpg)
![image](https://github.com/Pin-Yi-Judy/Data-Wrangling-Project/blob/main/Images/Trend%20Analysis.jpg)
**Draw Conclusion**
- Eyeball observes the correlation between movie ratings and tourist number

