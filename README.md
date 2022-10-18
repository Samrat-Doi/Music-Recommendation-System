# Spotify-Recommendation-System-With-Deployment<br><br>
## PLANNING<br>
In This Project We Build a Spotify Recommendation System as a Machine Learning application. Spotify is a digital music, podcast, and video service that gives us access to millions of songs using Spotify API.
### WHAT IS A RECOMMENDATION SYSTEM ?<br>
One of the most used machine learning algorithms is recommendation systems. A Recommendation System is a filtering system which aim is to predict a rating or preference a user would give to an item, eg. a film, a product, a song, etc.
- Spotify use different types of Recommendation Systems which are:
    - Collaborative Filtering Algorithm (Based on users interactions of different track)
    - Content Based Filtering (Based on users demographics and tracks attributes)
    - Natural Language Processing (Based on analyzing tracks lyrics).
- In this project we build our Recommendation System to recommend similar songs to the user input, based on:
    - Item-Item Based Collaborative Filtering
    - Clustering
## DATA<br>
Data came from 2 sources:
- API Calls of Spotify's Web API to get audio features for each track.
- The Spotify Million Playlist Dataset which contained Four separate JSON files

### UNDERSTANDING THE DATA:
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
    - Acousticness- A confidence measure from 0.0 to 1.0 of whether the track is acoustic or not.
    - Danceability- A value of 0.0 is least danceable and 1.0 is most danceable.
    - Duration_ms- The duration of the track in milliseconds.
    - Energy- It is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. 
    - Id-  Spotify ID for the track.
    - Instrumentalness- Measure of vocals in the track.
    - Key- Integers map to pitches using standard Pitch Class notation. E.g. 0 = C, 1 = C♯/D♭, 2 = D, and so on. If no key was detected, the value is -1.
    - Liveness- Detects the presence of an audience in the recording. A value above 0.8 provides strong likelihood that the track is live.
    - Loudness- It is the quality of a sound that is the primary psychological correlate of amplitude. Values typically range between -60 and 0 db.
    - Mode- Indicates the modality (major or minor) of a track. Major is represented by 1 and minor is 0.
    - Speechiness- It detects the presence of spoken words in a track. Values between 0 and 1.
    - Tempo- It is the speed or pace of a given piece and derives directly from the average beat duration.
    - Time_signature- It is the measure of each beat in a bar. The time signature ranges from 3 to 7 indicating time signatures of "3/4", to "7/4".
    - Valence- It is a measure of positivity with high valence sound more positive (e.g. happy, cheerful),low valence sound more negative (e.g. sad,angry).

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


## Software Development Stage<br>
## Model Building <br>
Clustering
 - K-Means or KNN 
 - We divided the tracks into clusters (as there are existed track genres) and recommend to the user according to cluster and popularity.
 - Very fast algorithm.

Choosing the features
 - Correlation is used on reducing the tracks dataset features to the most important one.
 - PCA is used on reducing the most important features into other pca components that maintains the highest variability of the data.

Item-Based Filtering
 - We used track features and created the track metadata feature which includes the the "artist", "album" and "track name" and got the mean of the similarity
 (using cosaine similarity) due to track features and track metadata feature and recommend to the user according to similarity and popularity.
 - It is highly accurate algorithm.

## Recommendations:
 - We can improve our item-based recommendation algorithm.
 - We can recommend the songs relative to the song name and artist name both, instead of just the song name to get more accurate result.

## TESTING STAGE
**Problem:** Our dataset doesn't has ratings feature
**Solution:** We calculated it based on another features

**Solution Details:**
    First: use number of dublicates of every track in play list as user rating
    Second: take the average of user interactions for every track (calcultaed in first step) weighted by the simmilarity Between this user and the target user

    Third: Calculate genre score:
        First: Cluster our tracks into 4 clusters
        Second: Calculate the percentage of this cluster in usr playlist
                (ex: a playlist has 10 tracks, 5 of them under cluster 1 and ther other five is under cluster 2)
        Third: for every recommended item we check its cluster and give it the percentage of this cluster in the target user playlist

    Fourth: Calculate the modernity score (apply MinMaxScaler on prduction_year)
    Fifth: you already has popularity score
    sixth: combine all the above together
    
    Final score = w1*interactions_score + w2*genre_score(cluster) + w3*modernity_score + w4*popularity_score + bias

### Types Of Evaluation Metrics
-  There are multiple evaluation metrics that can be used to measure accuracy of recommender system.
-  So, We should maximize all of them and solve the trade-off that happens between them, because some metrics are oposite to others but both of them is desired.

- *Classification Accuracy Metrics*   
- *Predictive Accuracy metrics*
- *Recommendation-centric metrics*
- *Personaization*

#### Classification Accuracy Metrics:
This type of metrics measures whether this recommender system can recommend correct tracks to correct user
The exact rating or ranking of objects is ignored
 
* Precision@k*: (num of top k recommendations that are relevant)/(num of items that are recommended)
      First, we look at two lists:
           first list: items that user interacted with
           second list: items that are recommended
      Second, we find the number of intersected items between the two lists
      Third, we divide it (number of intersected items) by the number of the **second** list

* Recall@k*: It is a fraction of top 'k' recommended items that are in a set of items relevant to the user.
      First we look at two lists:
          first list: items that user interacted with
          second list: items that are recommended
      Second we find the number of intersected items between the two lists
      Third we divide it (number of intersected items) by the number of the **first** list

* F1@k*: It's a formula that compines the above two metrics [Precision, recall]
      Denominator: inverse of summation (of the inverse of recall and precision) divided by  two
      First: we take the inverse of precision then the inverse of recall
      Second: take the summation of both of them (recall inverse + precision inverse)
      Third: divide this summation by two
      Finally: F1 score is the inverse of the result

![](https://www.researchgate.net/profile/Sebastian-Bittrich/publication/330174519/figure/fig1/AS:711883078258689@1546737560677/Confusion-matrix-Exemplified-CM-with-the-formulas-of-precision-PR-recall-RE.png)


*Matthews correlation coefficient (MCC)*: 
- It overcomes the drawbacks of F1 score.
- As we see in our calculations that F1 score doesn't consider True negative items.
- True negative is a term called for those items which recommender system does't put into his list because these items are not similar to the user.

    ![](https://miro.medium.com/max/1400/1*R6_BTaMSdCLdNBa0oauFQQ.png)

### Predictive Accuracy metrics:
    This type of metrics measures how ratings calculated by recommender system is close to user ratings

*Mean Absolute Error (MAE)*: The difference between what user actual rate to what our system predicts.
    First: you take the difference between actual and predictive rating for every all items of one user.
    Second: take the average of all these diferences. hence you have MAE for one user.
    Third: apply the above two steps on all users so that you have MAE for every user.
    Forth: take the average of all these user MAE

    ![](https://www.statisticshowto.com/wp-content/uploads/2016/10/MAE.png)

*Root mean square error (RMSE)*: 
   -It's same as MAE but it gives a large weight to large errors
    ![](https://miro.medium.com/max/966/1*lqDsPkfXPGen32Uem1PTNg.png)

*Normalized Mean Absolute Error (NMAE)*: 
- It normalize user ratings first
    First: calculate the average of ratings of every user
    Second: take the difference between rates and the average of this user
    Third: apply MAE on the result of second step

### Recommendation-centric metrics:
 - The target of these metrics is to ensure that items recommended by the recommender system has some certain characteristics.
 - Diversity: This metric wants to ensure two things:
    - Every recommended item is different form others.
    - Recommended item should be new item you the user so the user doesn't interact with before.
 - Coverage: 
    - Measures the ability of the recommender system to recommnd all the items dataset.
    - This measure is important because in some cases you may face that recommender systems doesn't recommend some items for any user.

### Personaization:
    This metric wants to ensure that these certain items are recommended to this specific user

    First: You apply recommender system on all your users
    Second: Concatenate all the recommended items to gether with no dublicates in one list
    Third: Concatenate all users in one list with no dublicates.
    Fourth: Create a DataFrame so that columns are the recommended items, index is user list
    Fifth: Fill this DataFrame by True or False (True means this item is recommended to this user)
    sixth: Calculate the disimilarity between users

    Note: a high personalization score indicates user’s recommendations are different, meaning the model is offering a personalized experience to each user.

## Deployment:
 - Flask
We create our app by using flask , then deployed it to Heroku.
 - Heroku
We deploy our flask app to Heroku.com. In this way, we can share our app on the internet with others. We prepared the needed files to deploy our app
successfully:
     -  Procfile: contains run statements for app file and setup.sh.
     -  requirements.txt: contains the libraries must be downloaded by Heroku to run app file (app.py) successfully
     -  model.py: contains the python code of the recommendation system algorithm.




