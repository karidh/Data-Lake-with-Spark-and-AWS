# Data lake Project

### Table of contents
> [Introduction](#Introduction)<br>
  [I. Project description](#Projectdescription)<br>
  [II. Project Datasets](#directories)<br>
  [III. Data Lake Implementation](#implementation)<br>
  [IV. Execution](#Execution)<br>

### Introduction
This project consist to use Spark python module to construct a Data lake for a music streaming startup nammed Sparkify.
As sparkify's users and songs database keep to grow, they want to have a new system to adapt that evoluation of data.

### I. Project description 
In this project we will use Spark to read all sparkify's log data, from an AWS S3 bucket, wrangling them  to extract all informations that the analytics team are need to do their analysis and saved these information into several parquets format. <br>
The parquets files that we created, would be use directely by the analytics team like natural database tables. So it will be easy to them to do their analysis.
 
### II. Project Datasets  

The datasets that we will use in this project have stored into two different S3 directory. Each of them contains several JSON log files in which are saved sparkify's activities log. Bellow the different directories:

#### 1. Song Data
* **s3://udacity-dend/song_data**:
The first dataset is **song_log**. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.
![Imgur](https://i.imgur.com/zi9933K.png)
And below is an example of what a single song file, ==TRAABJL12903CDCF1A.json==, looks like

        {"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0
    
#### 2. Log Data
* **s3://udacity-dend/log_data** The second dataset contains a simulation of the users's activties log. 
The log files in the dataset you'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.
![Imgur](https://i.imgur.com/tVeYup8.png)
And below is an example of what the data in a log file, ==2018-11-12-events.json==, looks like.
![Imgur](https://i.imgur.com/aw9wEBG.png?1)

### III. Data Lake Implementation
In this section we will present the different steps that we followed to build the data lake. 

#### 1. Schema for Song Play Analysis
With the different datasets we presented above, we will to create a star schema on song play analysis. This schema includes the following tables:

* **songplays (Fact table)**: records in log data associated with song plays i.e. records with page *NextSong*
    
        songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
    
* **users (Dim table)**: users in the app:
            
        user_id, first_name, last_name, gender, level
* **songs (Dim table)**: songs in music database
    
        song_id, title, artist_id, year, duration
* **artists** - artists in music database
         
        artist_id, name, location, lattitude, longitude

* **time** - timestamps of records in songplays broken down into specific units
        
        start_time, hour, day, week, month, year, weekday

#### 2. Spark initialisation
The first step of our data lake implementaion consited to initialyse Spark. Before to use Spark, we have to import spark into our project and make some configurations. As our implementation has done with python, we imported spark python module called *pyspark*. After that, we created a spark context that we used to build the data like.

#### 3. Data Wrangling
After initialyzed Spark into our project, we use it to read the different JSON files to extract the informations that we needed to build the star schema. As we can know, all data that we extract from the json files did not have a good format to permit us to build our schema. So we used spark to transform and fit them into the correct format. 

### IV. Project Structure

#### 1. Project files
The project contains three files and one directory.
* **etl.py**: It is the main file of implementation. It contains several that we defined to create the data lake.
*  **dl.conf**: contains AWS credential to connect to the S3 bucket
*  **Reamde.md** This is a currently file in which we describe the project.
In the directory there are a zip of the datasets and we use this directory to store the different parquet that we create.

#### 2. Presentation of etl.py
In this file we defiened four different fonctions. 
* In the first fonction named *create_spark_session* we wrote code to create spark session. 
* The second that we defined reads song-data json files from S3 bucket, extract and wrang data to create **songs.parquet** and **artist.parquet**. It named *process_song_data* and take three parameters: the spark session variable, the datasets path and the output path i.e the path where we will store the our parquet.
* The third fonction it like the previously function. We write the code to read log data from S3 and extract data to create **users.parquet**,**times.parquet**. Also we read the song.parquet that we create with the second function to joind with log data to create the **songsplay.parquet**.
* Finaly we defined the main function that runs the differents functions that we that presented above.

### IV. Execution

#### 1. Execution of etl.py
* First stage running
![Imgur](https://i.imgur.com/ZJ482Nm.png)

#### 2. Parquets created
* List of parquets
![Imgur](https://i.imgur.com/qm9YRtr.png)

* Parquet's Structure
**users parquet**:
    ![Imgur](https://i.imgur.com/snsjOI4.png)
 **artists parquet**:
    ![Imgur](https://i.imgur.com/kME4cf8.png)
**songs parquet:**
    ![Imgur](https://i.imgur.com/556diRv.png)
**times parquet**: 
![Imgur](https://i.imgur.com/gmclXj2.png)
**songplays parquet**: 
![Imgur](https://i.imgur.com/VyPwP3N.png)

#### 3. Example of queries
 * **Filter list of song where year is not 0**
    ![Imgur](https://i.imgur.com/QGjAIMD.png)
    
* **List of ten songplays records**  
![Imgur](https://i.imgur.com/qzMLDcO.png)


