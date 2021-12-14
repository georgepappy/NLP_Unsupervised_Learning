# PROJECT PROPOSAL

##### A CONTENT-BASED RECOMMENDER SYSTEM FOR TITLES IN THE TMDB MOVIE DATABASE

The TMDB.org movie database would like to create a "Movies You May Like" recommendation system for registered users to improve the user experience and hopefully drive up registrations. While many "power" users are already registered on the site, management would like to attract, retain and actively engage a much larger base of relatively novice-level users. This will hopfully take the site from its current status as a somewhat obscure operation towards one that rivals IMDB.com in terms of popularity and daily user base by growing an appreciatiation of the added recommendation functionality offered by TMDB.org.

Since TMDB hopes to capture new users with this feature, a key assumption is that many such users will have no (or few) stored ratings of their own which would enable a Collaborative Recommender System (matching the user's past movie ratings to similar users' ratings profiles). Therefore, the main approach of this project will be to build a Content-Based Recommender. However, if time permits, a Hybrid Recommender System may be developed which would allow use of TMDB's stored list of registered user ratings to combine Collaborative Recommendations with the baseline Content-Based system into a Hybrid Recommender system. 

The dataset for this project originated on TMDB.org but was downloaded from this source: https://grouplens.org/datasets/movielens/latest/ . It consists of 58,098 movie titles, each with a list of associated genres. Additionally, a companion dataset (downloaded from the same source) contains 10,747,027 user movie ratings (by uniquely-identified user) ranging from 0.0 to 5.0 (with 5.0 being best). 

Supporting datasets were generated using the TMDB API (https://www.themoviedb.org/documentation/api) to create lists of the top four actors, director, and overview (plot summary) for each of the movies in the first dataset mentioned above. Discarding rows with missing values and incomplete information, the final dataset used in this project will consist of 47,794 movies (rows), with the following columns:

- Movie ID (integer)        :  A unique integer identification number for a given movie
- Title (string)                  :  A movie title
- Genres (list of strings)  :  A list containing one or more genres associated with the movie title (e.g., "comedy")
- Overview (string)          :  A brief plot summary/description of the movie (25 - 1000 characters)
- Director (string)            :  The name of the movie's director
- Actors (list of strings)   :  A list of the four highest-billed actors in the movie

The aforementioned user movie ratings may be used (time permitting) as supplemental data for a Collaborative Recommender capability and consists of the following columns:

- User ID (integer)     :  A unique integer identification number for a given user
- Movie ID (integer)   :  A unique integer identification number for a given movie (matches the Movie ID described above)
- User Rating (float)   :  A user rating between 0.0 and 5.0 (with 5.0 being best)

As previously indicated, this project will use the described datasets to create a Recommender System which provides a ranked list of movie titles a given user may also like based on their most recent viewed movie. The baseline model will combine the words in all text columns (except Title) of the primary data, preprocessing the Overview text to remove stopwords, lemmatize and tokenize. These results will then be combined with the other text columns to form an overall bag of words which will be converted into a document-term matrix. Dimensionality reduction (PCA) will be applied to this matrix, and the results will be used to generate distances between movies which enable identification of similar movies based on relative degrees of similarity.

Various methods of lemmatization and vectorization (as well as variations within each method such as which n-grams to include, minimum and maximum document term frequencies to allow, etc.) will be tried to improve performance. Other variations will include using only some of the columns in the dataset for modeling (e.g., only the overviews, or only the lists of actors and director) and trying different distance metrics (Euclidean, Cosine Similarity, etc.)

The tools required for this project are: 

1. Pandas to clean, explore and generate the final modeling data
2. NLTK, SKLearn, and other libraries (e.g., spaCy, RAKE, etc.) to perform basic NLP tasks (removing stopwords, lemmatizing, tokenizing) and dimensionality reduction
3. Matplotlib and Seaborn to visualize the data and (if necessary/practical) final results
4. Python 3.8 (to be able to use all of the above)

The Minimum Viable Product for this project will be a report (and slides) presenting a working model with meaningful specific examples of recommender outputs demonstrating that pertinent movies are actually recommended and irrelevant movies are omitted from the outputs.

