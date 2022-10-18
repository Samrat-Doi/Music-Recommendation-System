# Spotify-Recommendation-System-With-Deployment<br><br>
## Planning Stage<br><br>
In This Project We Build a Spotify Recommendation System as a Machine Learning application. Spotify is a digital music, podcast, and video service that gives us access to millions of songs using Spotify API.
### What's A Recommendation System ?<br><br>
One of the most used machine learning algorithms is recommendation systems. A Recommendation System is a filtering system which aim is to predict a rating or preference a user would give to an item, eg. a film, a product, a song, etc.
- Spotify use different types of Recommendation Systems which are:
    - Collaborative Filtering Algorithm (Based on users interactions of different track)
    - Content Based Filtering (Based on users demographics and tracks attributes)
    - Natural Language Processing (Based on analyzing tracks lyrics).
- In this project we build our Recommendation System to recommend similar songs to the user input, based on:
    - Item-Item Based Collaborative Filtering
    - Clustering
    
## Data<br><br>
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
    
## Analysis of the data <br><br>

Let's start with the requirements and then go to the preprocessing stage:
Functional requirements:
The input will be the song name and the artist's name. The recommendation model goal/output is to find the top ten related songs to the input. It analyzes the input and then uses the provided Spotify dataset/API to get the output.
Non-functional requirements:
The model should obtain the highest accuracy possible by getting the most related songs to input. The model should be optimized to require as low computational power as possible. The model performance will be then evaluated using evaluation metrics. 

## Data Visualization <br><br>

"""insert  int_visualization_1.png   here""" 

In the above picture you can see the overall distribution of integer features of the dataset. By looking at the graph we can see that,
        - modified_at feature seems to be left skewed.
        - num_of_edits feature seems to be right skewed.
    After calculating the skewness of all the features, following result was obtained:
        Integer columns Skewness: 
            - 'pid': 0.017808960867489464,
            - 'modified_at': -1.5708263246583083,
            - 'num_tracks': 0.4061981401693917,
            - 'num_albums': 0.6532191347624093,
            - 'num_followers': 57.32408837141191,
            - 'num_edits': 1.7343797592157533,
            - 'playlist_duration_ms': 0.5236201051400035,
            - 'num_artists': 0.9187385911738083,
            - 'pos': 1.195362099841709,
            - 'track_duration_ms': 13.061421293930474
        Hence we can conclude by looking at the values as well that 'modified_at' and 'num_of_edits' feature is skewed and has to be normalized.

"""insert  float_visualization_1.png   here""" 

In the above picture you can see the overall distribution of float features of the dataset. By looking at the graph we can see that,
        - 'speechiness' feature seems to be right skewed
        - 'liveness' feature seems to be right skewed
    After calculating the skewness of all the features, following result was obtained:
        Float columns Skewness: 
            - 'album_total_tracks': 6.593166804845857,
            - 'popularity': -0.04023786418535345,
            - 'danceability': -0.307191329691003,
            - 'energy': -0.6116277854029948,
            - 'key': 0.03388847864952774,
            - 'loudness': -2.101493692230863,
            - 'mode': -0.6913052307054481,
            - 'speechiness': 2.5464790626851856,
            - 'acousticness': 1.2656000249813504,
            - 'instrumentalness': 3.7201500426519707,
            - 'liveness': 2.2136494725426727,
            - 'valence': 0.09932104654760834,
            - 'tempo': 0.3503231864768454,
            - 'time_signature': -4.652308881089511
        Hence we can conclude by looking at the values as well that 'speechiness' and 'liveness' feature is skewed and has to be normalized.

"""insert comparision_visualization_3.png here"""

In the above picture you can see the distribution of skewed features before and after applying the transformation methods.
        - power of 1/4 was applied to right skewed features.
        - cube root was applied to left skewed features.

"""insert correlation_visualization_4.png here"""

        --------------------------------------------------------------------
        Strong Positive Co-Relations between Features with threshold =  0.5

            ('num_albums', 'num_tracks', 0.8415447166107518),
            ('num_edits', 'num_tracks', 0.5299566460316026),
            ('num_edits', 'num_albums', 0.6163409150659508),
            ('playlist_duration_ms', 'num_tracks', 0.977662549636656),
            ('playlist_duration_ms', 'num_albums', 0.8319682926543831),
            ('playlist_duration_ms', 'num_edits', 0.5237533705040985),
            ('num_artists', 'num_tracks', 0.7269838343923531),
            ('num_artists', 'num_albums', 0.9355542001527783),
            ('num_artists', 'num_edits', 0.5776211268126814),
            ('num_artists', 'playlist_duration_ms', 0.7152958361571978),
            ('pos', 'num_tracks', 0.6524721120620393),
            ('pos', 'num_albums', 0.5490635615366086),
            ('pos', 'playlist_duration_ms', 0.6378924301360763),
            ('loudness', 'energy', 0.7507065639450008)

        --------------------------------------------------------------------
        Strong Negative Co-Relations between Features with threshold =  -0.5

            ('acousticness', 'energy', -0.6588128400948912),
            ('acousticness', 'loudness', -0.553673956621521)
