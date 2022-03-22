## Data Warehousing with AWS Redshift


#### Project Description
A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud.
The data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.
In this project, I have built an ETL pipeline that extracts the data from S3, stages them in Redshift, and transforms data into a set of dimensional tables.
The analytics team can then access the processed data to find insights into what songs their users are listening


#### Datasets
Datasets used in this project resides in pulic access S3 bucket.Below are the S3 links for each:\
      Song data: s3://udacity-dend/song_data\
      Log data: s3://udacity-dend/log_data\
The datasets are JSON files namely song_data and log_data

###### Song Dataset
The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are file paths to two files in this dataset.\
song_data/A/B/C/TRABCEI128F424C983.json\
song_data/A/A/B/TRAABJL12903CDCF1A.json

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.\
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

###### Log Dataset
The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.

The log files are partitioned by year and month. For example, here are filepaths to two files in this dataset.\
log_data/2018/11/2018-11-12-events.json\
log_data/2018/11/2018-11-13-events.json
song_data holds the details of the song and its artists\
log_data hold the details of users which tell what songs is being heard and others

And below is an example of what the data in a log file, 2018-11-12-events.json, looks like.
![alt text](https://github.com/rumijha/data-modeling-with-postgres/blob/main/log-data.png)


#### Database Design

#### Staging Tables
**staging_events_table_create** - Hold the data of the log dataset on Redshift
* *artist, auth, firstName, gender, iteminSession, lastName, length, level, location, method, page, registration, sessionid, song, status, ts, userAgent, userid*

**staging_songs_table_create** - Holds the data of the song dataset on Redshift
* *num_songs, artist_id, artist_latitude, artist_longitude, artist_location, artist_name, song_id, title, duration, year*

#### Fact Table
**songplays** - records in log data associated with song plays i.e. records with page NextSong
* *songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent*

#### Dimension Tables
**users** - users in the app
* *user_id, first_name, last_name, gender, level*

**songs** - songs in music database
* *song_id, title, artist_id, year, duration*

**artists** - artists in music database
* *artist_id, name, location, latitude, longitude*

**time** - timestamps of records in songplays broken down into specific units
* *start_time, hour, day, week, month, year, weekday*


#### Project Files
In addition to the data files, the project includes four files:
* *create_tables.py* drops and creates the tables. This file can be run to reset the tables each time before the ETL scripts are run.
* *etl.py* load data from S3 to staging tables on Redshift and load data from staging tables to dimensions tables on Redshift.
* *sql_queries.py* contains all the sql queries, and is imported into the two files above.
* *dwh.cfg* contains redshift database and IAM role information.


#### How To Run the Project
Run etl.py to process all the files under S3 and load the data into Redshift staging tables. Accessing staging tables to load data into respective dimension tables in redshift.
