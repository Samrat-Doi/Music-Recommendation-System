# Spotify-Recommendation-System-With-Deployment<br><br>
## Planning Stage<br>
In This Project We Build a Spotify Recommendation System as a Machine Learning application. Spotify is a digital music, podcast, and video service that gives us access to millions of songs using Spotify API.
### What's A Recommendation System ?<br>
One of the most used machine learning algorithms is recommendation systems. A Recommendation System is a filtering system which aim is to predict a rating or preference a user would give to an item, eg. a film, a product, a song, etc.
- Spotify use different types of Recommendation Systems which are:
    - Collaborative Filtering Algorithm (Based on users interactions of different track)
    - Content Based Filtering (Based on users demographics and tracks attributes)
    - Natural Language Processing (Based on analyzing tracks lyrics).
- In this project we build our Recommendation System to recommend similar songs to the user input, based on:
    - Item-Item Based Collaborative Filtering
    - Clustering
## Data<br>
Data came from 2 sources:
- API Calls of Spotify's Web API to get audio features for each track.
- The Spotify Million Playlist Dataset which contained Four separate JSON files

### Understanding the Dataset:
- The Spotify million playlist dataset consists of 4 JSON Files:
    - pid - the playlist ID.
    - name - (optional) - the name of the playlist. For some challenge playlists, the name will be missing.
    - tracks - a (possibly empty) array of tracks that are in the playlist. Each element of this array contains the following fields:
         * pos - the position of the track in the playlist (zero offset)
         * track_name - the name of the track
         * track_uri - the Spotify URI of the track
         * artist_name - the name of the primary artist of the track
         * artist_uri - the Spotify URI of the primary artist of the track
         * album_name - the name of the album that the track is on
         * album_uri -- the Spotify URI of the album that the track is on
         * duration_ms - the duration of the track in milliseconds
    - num_tracks - the total number of tracks in the playlist.
- The Audio Features are scraped from Spotify API using spotipy library, which are:
    - Acousticness: A confidence measure from 0.0 to 1.0 of whether the track is acoustic or not.
    - Danceability: A value of 0.0 is least danceable and 1.0 is most danceable.
    - Duration_ms: The duration of the track in milliseconds.
    - Energy: It is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. 
    - Id:  Spotify ID for the track.
    - Instrumentalness: Measure of vocals in the track.
    - Key: Integers map to pitches using standard Pitch Class notation. E.g. 0 = C, 1 = C♯/D♭, 2 = D, and so on. If no key was detected, the value is -1.
    - Liveness: Detects the presence of an audience in the recording. A value above 0.8 provides strong likelihood that the track is live.
    - Loudness: It is the quality of a sound that is the primary psychological correlate of amplitude. Values typically range between -60 and 0 db.
    - Mode: Indicates the modality (major or minor) of a track. Major is represented by 1 and minor is 0.
    - Speechiness: It detects the presence of spoken words in a track. Values between 0 and 1.
    - Tempo: It is the speed or pace of a given piece and derives directly from the average beat duration.
    - Time_signature: It is the measure of each beat in a bar. The time signature ranges from 3 to 7 indicating time signatures of "3/4", to "7/4".
    - Valence: It is a measure of positivity with high valence sound more positive (e.g. happy, cheerful),low valence sound more negative (e.g. sad,angry).

## Data Cleaning <br>
- Extracted data had variables like "Description" which had more than 40% null values. Hence, they were dropped. 
- Features with less than 40% null values were filled with median values in continous value type features where as that of mode values in object-type features.
- Duplicates rows were identified and dropped from the dataset.

## EXPLORATORY DATA ANALYSIS <br>
"""insert  int_visualization_1.png   here""" 
-As per the above graphs:
   - "modified_at" feature seems to be left skewed.
   - "num_of_edits","liveness" and "speechiness" feature are right skewed.
 *Hence we can conclude, these feature need to be normalized.*
 
"""insert comparision_visualization_3.png here"""
-In the above picture you can see the distribution of skewed features before and after applying the transformation methods.
   - power of 1/4 was applied to right skewed features.
   - cube root was applied to left skewed features.

"""insert correlation_visualization_4.png here"""
-Strong Negative Co-Relations between Features with threshold =  -0.5
*Hence, 'acousticness' is negatively correlated with 'energy' and 'loudness'.*

## DESIGN AND PROTOTYPE
Let's start with the requirements:
    -Functional requirements:
        * The input will be the song name and the artist's name. The recommendation model goal/output is to find the top ten related songs to the input. 
        * It analyzes the input and then uses the provided Spotify dataset/API to get the output.
    -Non-functional requirements:
        * The model should obtain the highest accuracy possible by getting the most related songs to input. 
        * The model should be optimized to require as low computational power as possible. The model performance will be then evaluated using evaluation metrics. 


# Software Development Stage.
## Model Building:

Clustering
 - We divided the tracks into clusters (as there are existed track genres) and recommend to the user according to cluster and popularity.
 - Very fast algorithm

Choosing the features
 - Correlation is used on reducing the tracks dataset features to the most important one.
 - PCA is used on reducing the most important features into other pca components that maintains the highest variability of the data.

Content-Based Filtering
 - We used track features and created the track metadata feature which includes the the artist + album + track name and got the mean of the similarity
 (using cosaine similarity) due to track features and track metadata feature. and recommend to the user according to similarity and popularity.
 - As it takes 20 minutes to iterate over all the tracks
 - Very accurate algorithm

## Recommendations:
 - We can improve our content-based recommendation algorithm.
 - We can recommend the songs relative to the song name, artist instead of the song name only to get more accurate result.

## Deployment:
 - Flask
We create our app by using flask , then deployed it to Heroku.
 - Heroku
We deploy our flask app to Heroku.com. In this way, we can share our app on the internet with others. We prepared the needed files to deploy our app
successfully:
     -  Procfile: contains run statements for app file and setup.sh.
     -  requirements.txt: contains the libraries must be downloaded by Heroku to run app file (app.py) successfully
     -  model.py: contains the python code of the recommendation system algorithm.




