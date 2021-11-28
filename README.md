# ML-Project
Music is a great stress buster, helps in relaxation, and elevates our mood. Songs have different lyrics and sentiments attached, unique audio features which leave an impact on the audience. While some songs become a sensation, some fail to leave their mark. With so many new artists coming up, releasing a top-of-the-chart song is all the more difficult.

There have been different studies to predict the success of a song based on its audio features, lyrics, and emotion. Some researchers have also tried to study the artist's impact, releasing date, and other metadata surrounding the song. In this project, we aim to study the impact of acoustic features in predicting a song's popularity without any information on the artist. We also present a new dataset containing information only on Indian Songs; analyze the difference in the impact of acoustic features on song popularity in India as opposed to globally.

## Acoustic Features
Acoustic features of audio are the physical features that can be recorded and analyzed. They include but are not limited to loudness, valence, tempo, and danceability. Many studies have found a correlation between these acoustic features and song popularity.  This project proposes classifying songs as Successful (Hit Song) or Unsuccessful (Unpopular Song) based mainly on acoustic features. Past studies show that the artist's information has a significant effect on song popularity. However, our focus is only to analyze and study the impact of acoustic features on song popularity when the song's artist is not known. This will be helpful, especially for newer artists who do not have any previous hits and want to predict the success of a song based on its physical characteristics.

## Dataset
The dataset contains some metadata and various audio features about the songs. The Meta-Informative features include track id, track name, artist name and album name. The Acoustic features include the measure of acousticness, dancebility, energy, loudness among others.

### Data Extraction and Preprocessing
We used the Spotipy library to access the Spotify API for song information retrieval. We obtained information of 70,000+ songs using different queries based on genre,year and with the help of different public playlists. However, as Spotify has restricted the offset size to 1000 recently, we were not able to extract more songs using a query. Hence, we decided to merge our dataset with the publicly available dataset. For preparing the dataset for the model, we removed all the features(fields) which were unique to the song (such as Song ID, Song Name) and information related to the artist. We initially gathered a dataset of 300,000+ entries however, there were many duplicates. We dropped all the duplicate entries using the pandas library. Also, we analyzed the dataset and realized a huge chunk (around 30\%) of the songs in the dataset had 0 popularity. This was because either the songs were released by unknown artists and were never publicised or due to insufficient data. To ensure a normal distribution of our dataset, we decided to drop these songs. Further, we visualized the distribution in popularity of songs against all the features and removed all the extreme outliers like songs of duration more than 8 minutes. We decided to consider only the 'year' parameter of release data in order to avoid complexity while training. We normalized the audio features using min-max normalization on the column. This normalization technique is quite useful to avoid the biases of certain features with respect to others during model training. We then used this normalized data for feature extraction and model training. After the preprocessing stage, we were left with 118,795 different songs which were composed by 14,222 different artists.

### Indian Dataset
For our analysis on Indian Songs, we collected data on songs made by Indian Artists by querying track information based on 'Bollywood', 'India Indie', 'Tollywood' genres and by fetching online playlists consisting of songs by Indian Artists. We also collected data from our original dataset, whose artists were Indian. Following preprocessing techniques similar to our original dataset, we were able to extract 11,844 different songs by 1,394 different artists.

## Feature Selection
As the dataset had a lower dimension, we decided to perform feature selection instead of feature extraction. We used the following methods to distinguish which features greatly affected the predictions of the model and which features did not play a great role and could be dropped from the dataset.

### Fischer's score
It is a supervised algorithm that returns the rank of each feature based on the fisher’s score. This rank can be used for feature selection among different variables. The higher the rank of the variable, the more useful is the feature in predicting the target variable.

### Information Gain
Information gain for each variable is calculated in the context of the target variable and is used for feature selection. It is calculated by subtracting the weighted entropy for each variable from the original entropy. The higher the information gain, the greater is the decrease in entropy.

### Correlation Coefficient
It is a measure of the linear relationship between 2 or more variables. It helps in predicting a variable based on the value of another variable. It helps in deciding the features which are largely correlated with each other and can be dropped after determining their correlation with the target variable and therefore help in feature selection.

### Inference and Final Data
Therefore, after observing the results of all the above techniques we arrived at the conclusion to drop the features 'Mode', 'Key' and 'Time Signature'. The reason is that the features 'Mode', 'Key' and 'Time Signature' are providing relatively lower information as compared to other features. Also, it can be observed that the Fischer score value for 'Mode' and 'Time Signature' is relatively low. It can be also seen that a relatively lower value of Fischer score is obtained for the features 'Liveliness' and 'Loudness' but one of the reasons for this could be that as the computational complexity for Fischer score is relatively high for large amount of data, therefore we performed the Fischer score technique on a smaller subset of data. Since, in all the other techniques the features 'Loudness' and 'Liveliness' provides a good amount of information, therefore we decided not to drop these features. The data used for the regression and classification problems are thus different. The regression models use a custom data(extracted from spotipy) after the feature selection process which has a 'Popularity' attribute which refers to a popularity score. On the other hand, the classification data make use of a dataset from Github which have a 'target' attribute signifying hit/loss.
