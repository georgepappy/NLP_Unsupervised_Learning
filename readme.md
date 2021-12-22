# UNSUPERVISED LEARNING/NLP PROJECT WRITE-UP

### A HYBRID RECOMMENDER SYSTEM FOR TITLES IN THE TMDB MOVIE DATABASE

## Abstract

The TMDB.org movie database would like to create a "Movies You May Like" recommendation system for registered users to improve the user experience and hopefully drive up registrations. While many "power" users are already registered on the site, management would like to attract, retain and actively engage a much larger base of relatively novice-level users. This will hopfully take the site from its current status as a somewhat obscure operation towards one that rivals IMDB.com in terms of popularity and daily user engagement by growing an appreciatiation of the superior user experience and functionality offered by TMDB.org.

Since TMDB hopes to capture new users with this feature, a key assumption is that many such users will have no (or few) stored ratings of their own which would enable a Collaborative Recommender System (matching the user's past movie ratings to similar users' ratings profiles). Therefore, the initial focus of this project was building a Content-Based Recommender. Once this had been achieved, a broader Hybrid Recommender System was developed which allows use of TMDB's stored database of registered user ratings to add Collaborative Recommendations to those provided with just the Content-Based system.

## Design

For the Content-Based Recommender, movie similarity was based on consideration of each movie's genre(s), director, four highest-billed actors and text-based plot summary (ranging from 26 to 1000 characeters). This data was tokenized, vectorized and dimension-reduced for each movie, and a cosine similarity calculation determined the degree of similarity between each movie in the database and every other (47,723 titles in total).

The Collaborative Recommender portion of the system was designed to apply only with users who have rated at least 5 movies, drawing from the highly-rated movies of other users who have rated at least 5 of the user under consideration's movies. A dot product calculation for each such user identifies the "most similar user" (based on past common ratings) and recommends movies that the most similar user has rated highly and which the current user has not yet rated.

In terms of Hybrid Recommender functionality, the decision was made to offer Content-Based Recommendations to all users and to simply also offer Collaborative Recommendations for users with at least 5 rated movies. Thus, this latter group of users essentially gets two lists of recommendations instead of just one. The rationale for this is that sometimes a user simple wants to view more movies like a certain title, while at other times they may want less-than-obvious recommendations (i.e. movies that may not be directly related to a certain title but which tend to be liked by other users who like the same movies as the current user).

## Data

The primary dataset for this project originated on TMDB.org but was downloaded from this source: https://grouplens.org/datasets/movielens/latest/ . It consists of 58,098 movie titles, each with a list of associated genres. 

Supporting datasets were generated using the TMDB API (https://www.themoviedb.org/documentation/api) to create lists of the top four actors, director, and overview (plot summary) for each of the movies in the first dataset mentioned above. Discarding rows with missing values and incomplete information, the final dataset used in this project had 47,723 movies (rows). This data became the basis for the Content-Based Recommender and consisted of the following columns:

- Movie ID (integer)        :  A unique integer identification number for a given movie
- Title (string)                  :  A movie title
- Genres (list of strings)  :  A list containing one or more genres associated with the movie title (e.g., "comedy")
- Overview (string)          :  A brief plot summary/description of the movie (26 - 1000 characters)
- Director (string)            :  The name of the movie's director
- Actors (list of strings)   :  A list of the four highest-billed actors in the movie

For the Collaborative Recommender, a companion dataset was downloaded from the same source cited above. It contained 10,747,027 user movie ratings (by uniquely-identified user, of which there were 110,490). For practical considerations, the dataset was reduced to just 50,000 unique users and 5,004,591 movie ratings). This data consisted of the following columns:

- User ID (integer)     :  A unique integer identification number for a given user
- Movie ID (integer)   :  A unique integer identification number for a given movie (matches the Movie ID described above)
- User Rating (float)   :  A user rating between 0.5 and 5.0 (with 5.0 being best, in increments of 0.5)

## Algorithms

The final Content-Based Recommender used spaCy to perform data cleaning and English stopword removal. Gensim was used to find all bi-grams and tri-grams occurring at least twice across all movies from the Plot Summaries. A Gensim Latetent Semantic Indexing (LSI) model was then used to generate topics from combinations drawn out of the dataset's bag of words. Examining a Coherence plot across different numbers of topics established that 3000 topics was an appropriate choice for the LSI model. Gensim's SparseMatrixSimilarity function was applied to this model. The resulting 47,723 x 3000 cosine similarity matrix formed the basis of the final Content-Based Recommender.

The Collaborative Recommender was created by generating a 50,000 x 28,044 User-Ratings matrix and using Truncated SVD to reduce it down to a 50,000 x 700 U matrix (retaining 71.1% explained variance). This U matrix was then used as the input for a function that computes dot products to determine degree of similarity between users who have rated at least 5 of the same movies, forming the basis for collaborative recommendations. 

## Tools 

The following tools were used in this project:

1. Pandas to clean, explore and generate the final modeling data, and to view recommendations
2. NLTK, SKLearn, spaCy and Gensim to perform basic NLP tasks (removing stopwords, tokenizing, LSI Topic Modeling) and dimensionality reduction
3. Matplotlib and Seaborn to visualize the data and dimension reduction results
4. Python 3.8 (to be able to use all of the above)

## Communication

In addtition to presenting final Project Slides to the stakeholders, all work (including the slides) can be found on GitHub: https://github.com/georgepappy/NLP_Unsupervised_Learning

