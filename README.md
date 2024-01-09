# Data Modeling with Postgres

## Introduction

The purpose of this database is to support the analytical goals of Sparkify, a music-streaming startup. By analyzing the data collected on songs and user activity, Sparkify aims to gain insights into what songs their users are listening to. This analysis can help them make data-driven decisions to improve their music streaming app, personalize recommendations, understand user preferences, and optimize their content library.


## Justification for the database schema and ETL pipeline:



* ETL Pipeline:

The ETL pipeline is responsible for extracting data from the source files, transforming it into the appropriate format, and loading it into the respective tables in the database.

- For the song dataset, the pipeline would extract the JSON metadata on songs, transform it into the desired schema, and load it into the "songs" and "artists" tables.

- For the log dataset, the pipeline would extract the JSON logs on user activity, transform and enrich the data with additional information, and load it into the "time," "users," and "songplays" tables.

The ETL pipeline ensures data consistency, integrity, and completeness in the database, allowing Sparkify to perform accurate and meaningful analyses.
Overall, the star schema design and ETL pipeline provide a structured and efficient approach to support Sparkify's song play analysis requirements, enabling them to gain insights into user behavior and music preferences and optimize their music streaming app accordingly.






## Prerequisites

This project makes the folowing assumptions:

* Python 3 is available
* `pandas` and `psycopg2` are available
* A PosgreSQL database is available on localhost

## Running the Python Scripts

At the terminal:

1. ```python create_tables.py```
2. ```python etl.py```

In IPython:

1. ```run create_tables.py```
2. ```run etl.py```

## Database Schema

The database schema design follows a star schema optimized for query performance and analysis. It consists of one fact table, "songplays," and multiple dimension tables, namely "users," "songs," "artists," and "time." This design enables efficient and straightforward querying of song play data and allows easy aggregation and analysis across different dimensions.

<img src="song play.png" alt="ERD Diagram" width="800"/>

* Fact Table:

- The "songplays" table is the star schema's central fact table. It captures the log data associated with song plays and serves as the primary focus for analysis.

- The "songplay_id" is the primary key to uniquely identifying each song play record.
The foreign keys in the "songplays" table, such as <br>
    -"user_id," 
    -"song_id,"
    - "start_time," and 
    - "artist_id," 

establish relationships with the dimension tables and enable joining and aggregating data from different perspectives.

* Dimension Tables:

The dimension tables provide additional context and details related to the song plays.

- The "users" table captures user information such as their first and last names, gender, and level (subscription type).

- The "songs" table contains details about the songs, including the title, artist_id, year, and duration.

- The "artists" table stores information about the artists, such as their name, location, and geographical coordinates.

- The "time" table breaks down the timestamp of the song plays into specific units like hour, day, week, month, year, and weekday.

Having separate dimension tables allows for efficient storage, eliminates data redundancy, and enables easier and faster analysis by avoiding complex joins and aggregations.

## ETL Process

The ETL pipeline is responsible for extracting data from the source files, transforming it into the appropriate format, and loading it into the respective tables in the database.

### Song Dataset

For the song dataset, the pipeline would extract the JSON metadata on songs, transform it into the desired schema, and load it into the "songs" and "artists" tables.

For example, here are filepaths to two files in this dataset.

```
song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json
```

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

```
{
    "num_songs": 1, 
    "artist_id": "ARD7TVE1187B99BFB1", 
    "artist_latitude": null, 
    "artist_longitude": null, 
    "artist_location": "California - LA", 
    "artist_name": "Casual", 
    "song_id": "SOMZWCG12A8C13C480", 
    "title": "I Didn't Mean To", 
    "duration": 218.93179, 
    "year": 0
}
```


### Log Dataset

- For the log dataset, the pipeline would extract the JSON logs on user activity, transform and enrich the data with additional information, and load it into the "time," "users," and "songplays" tables.
For example, here are filepaths to two files in this dataset.

```
log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json
```

This data contains information of which songs Users listened to at a specific time. Information is parsed to provide data for the Songplays Fact table and the Users and Time Dimension tables. The ```songplays.artist_id``` and ```songplays.song_id``` columns are populated by a lookup based on the Song Title, Artist Name and song Duration.

## Description of Files

### Directory: data/log_data

This directory contains a collection of JSON log files. These files are used to populate our Fact table - Song Plays - and to populate the Dimension tables for Users and Time.

### Directory: data/song_data

This directory contains a collection of Song JSON files. These files are used to populate Dimension tables for Songs and Artists.

## create_tables.py

This Python script recreates the database and tables used to storethe data.

## etl.ipynb

A Python Jupyter Notebook that was used to initially explore the data and test the ETL process.

## etl.py

This Python script reads in the Log and Song data files, processes and inserts data into the database.

## requirements.txt

A list of Python modules used by this project.

## sql_queries.py

A Python script that defines all the SQL statements used by this project.

## test.ipynb

A Python Jupyter Notebook that was used to test that data was loaded p
