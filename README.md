# Spotify-Recommendation-System-With-Deployment<br><br>

## Planning Stage<br><br>
In This Project We Build a Spotify Recommendation System as a Machine Learning application

as We Know Spotify is a digital music, podcast, and video service that gives us access to millions of songs and Our Planning 
To build a Spotify Recommender System Using Spotify API.

### What's A Recommendation System ?<br><br>
One of the most used machine learning algorithms is recommendation systems. A recommender (or recommendation) system (or engine) is a filtering system which aim is to predict a rating or preference a user would give to an item, eg. a film, a product, a song, etc.

- Spotify use different types of Recommendation Systems which are:
    - Collaborative Filtering Algorithm (Based on users interactions of different track)
    - Content Based Filtering (Based on users demographics and tracks attributes)
    - Natural Language Processing (Based on analyzing tracks lyrics).
    
- In this project we build our Recommendation System to recommend similar songs to the user input, we used different algorithms which are:
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
    - acousticness (A confidence measure from 0.0 to 1.0 of whether the track is acoustic. 1.0 represents high confidence the track is acoustic.)
    - analysis_url (A URL to access the full audio analysis of this track. An access token is required to access this data.)
    - danceability (Danceability describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength,     and overall regularity. A value of 0.0 is least danceable and 1.0 is most danceable.)
    - duration_ms (The duration of the track in milliseconds.)
    - energy (Energy is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy. For example, death metal has high energy, while a Bach prelude scores low on the scale. Perceptual features contributing to this attribute include dynamic range,      perceived loudness, timbre, onset rate, and general entropy.)
id (The Spotify ID for the track.)
    - instrumentalness (Predicts whether a track contains no vocals. "Ooh" and "aah" sounds are treated as instrumental in this context. Rap or spoken word tracks are clearly "vocal". The closer the instrumentalness value is to 1.0, the greater likelihood the track contains no vocal content. Values above 0.5 are intended to represent instrumental tracks, but confidence is higher as the value approaches 1.0.)
    - key (The key the track is in. Integers map to pitches using standard Pitch Class notation. E.g. 0 = C, 1 = C♯/D♭, 2 = D, and so on. If no key was detected, the value is -1.)
    - liveness (Detects the presence of an audience in the recording. Higher liveness values represent an increased probability that the track was performed live. A value above 0.8 provides strong likelihood that the track is live.)
    - loudness (The overall loudness of a track in decibels (dB). Loudness values are averaged across the entire track and are useful for comparing relative loudness of tracks. Loudness is the quality of a sound that is the primary psychological correlate of physical strength (amplitude). Values typically range between -60 and 0 db.)
    - Mode (indicates the modality (major or minor) of a track, the type of scale from which its melodic content is derived. Major is represented by 1 and minor is 0.)
    - speechiness (Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value. Values above 0.66 describe tracks that are probably made entirely of spoken words. Values between 0.33 and 0.66 describe tracks that may contain both music and speech, either in sections or layered, including such cases as rap music. Values below 0.33 most likely represent music and other non-speech-like tracks.)
    - tempo (The overall estimated tempo of a track in beats per minute (BPM). In musical terminology, tempo is the speed or pace of a given piece and derives directly from the average beat duration.
    - time_signature (An estimated time signature. The time signature (meter) is a notational convention to specify how many beats are in each bar (or measure). The time signature ranges from 3 to 7 indicating time signatures of "3/4", to "7/4".)
    - valence (A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).)
    




