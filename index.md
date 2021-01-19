![Movies](/images/movies.jpg){: .center-image_b}


This article is about how to predict genres based on the plot description of movies with Python. By using multi-label classification methods in Machine Learning, we can create several different models that does this for us!

Firstly, we need a dataset! The dataset that I will use can be found at [this link](https://www.kaggle.com/stefanoleone992/imdb-extensive-dataset) which is an extensive collection of movies from IMDb. We will use the title, plot and genre parameters from the "IMDb movies.csv" file. There are in total 83740 movies in this dataset.

Since movies can have multiple genres, like for instance [Drama, Comedy] the classification will be a multi-label problem where multiple labels may be assigned to each instance. This is not a multi-class problem though as the classes are not mutually exclusive. A good way to describe the difference between them is that multi-class classification makes the assumption that each sample is assigned to one and only one label: a fruit can be either an apple or a pear but not both at the same time. While in multi-label classification, the fruit can be both an apple and a pear, or none of them. In the context of movie genres, the latter makes more sense.

## Preprocessing

In order to be able to use the dataset, we need to structure the dataset into a managable datastructure and clean up its content. The first thing that can be done is to remove empty/null rows in the dataset. Then we can start cleaning the plot description. Before any preprocessing we see in the wordcloud below, which represents the most frequent words in the text by the size of the word, that the words are not related to movies at all. 

![Word Cloud](/images/wordcloud_without_pre.png){: .center-image}

Here, there are several methods that can be used, first off is the removal of stopwords. The stopwords are words like "it" and "the" etc. The most essential stopwords can be taken from the Natural Language Toolkit(NLTK) library which can be complemented with other words that are deemed necessary. 

This is followed by transforming all text to lower-case and removing everything that isn't a letter in the alphabet.
![Word Cloud2](/images/prepros.png){: .center-image_b}

The next step is to apply stemming to the text. Stemming is the process of reducing words to their word stem, i.e the most basic form of the word. If the word is "running" then it will be reduced to "run" for example. There are different ways to do stemming, this time we'll use the SnowballStemmer from NLTK.

This resulted in the wordcloud below which gives a better representation of movies than the one before. 

![Word Cloud2](/images/wordcloud2.png){: .center-image}


The whole dataset contains 25 genres which is quite a lot. The representation of each genre for all movies is very different where some genres only appear a couple of times, compared to "Drama" which was represented 46127 times. Thus, I removed all genres that was represented less than 3000 times. This left us 12 different genres which should be more than enough. The final distribution of movies is shown in the diagram below.

![Word Cloud](/images/genres_graph.png){: .center-image_b}

## Deciding what model to use!
At this point, the genres are structed as arrays of strings like [Drama, Comedy, Action] for each movie. These arrays need to be binarized! We can do that with the MultiLabelBinarizer which results in a binary 12x1 array instead.

Now we need to split our data into a train- and test set.

![Word Cloud2](/images/test_train.png){: .center-image}


These sets are converted to features with the TF-IDF technique, followed by the Machine Learning Model in the Pipeline.
![Word Cloud2](/images/PipelineOnevsRest.png){: .center-image_b}
![Word Cloud2](/images/PipelineLabelPower.png){: .center-image_b}
![Word Cloud2](/images/PipelineBinRel.png){: .center-image_b}

We fit the pipeline to the train data and predict the result with the test data.

Not all models may be suited for this multi-label classification problem. I tested OneVsRest, LabelPower, BinaryRelevance and Topic Modeling. I used GridSearchCV to optimize the parameters for each of them before comparing them.

### Models
I thought it would be interesting to see how many times each genre is predicted compared to the actual data before looking at the score.

![Word Cloud](/images/logisticreg.png){: .center-image_b}

OneVsRest had an okay distribution when comparing the number of predicted genres compared to the real values.

![Word Cloud](/images/LabelPower.png){: .center-image_b}

Labelpower was worse at predicting small sample genres but was pretty similar to OneVsRest otherwise.

![Word Cloud](/images/BinaryRel.png){: .center-image_b}

BinaryRelevance had a really wierd way of predicting as it chose too many genres overall.

When looking at how many genres that were predicted per movie, this one was guessing up to 12 (all genres available) genres per movie compared to the actual 1-4 genres. The OneVsRest classifier was the most accurate here.

I also tested Topic Modelling with 10 topics with the LDA. Since this resulted in a relative poor result, I did not continue with that method though.

### Scores
Finally, we get the scores for the different models.
![Word Cloud2](/images/scoreprint.png){: .center-image_b}

![Word Cloud](/images/scores3.png){: .center-image_b} 


The scores were in favor of OnevsRest and close second LabelPower. LabelPower had a slightly higher accuracy score while OnevsRest got a higher F1-score but LabelPower was really slow compared to OnevsRest.


## Improving the score even further
I continued with trying to improve the score of OnevsRest which got the F1-score: 53.14 and Accuracy score: 20.6. 

The prediction for a movie is represented by an array of scores between 0-1 for all the 12 genres. 1 represent a genre that true and 0 false. The threshold I chose was 0.5 where scores above that value will be considered as 1 and under 0. 

Many movies ended up with no predicted genres at all. So I tried to get these movies to have at least one genre each. A simple way of doing this was to just change the greatest value in the array to one. This resulted in the F1-score of 55.3 and Accuracy score of 22.6 which is a decent increase with little effort.

## Conclusion

Not the best result

the plots only consist of 1-2 sentences which might be too little for a precise result.

Many genres might overlap, atleast for me they do, drama to me is just the genre of when there is no good genre to describe the movie...

might be another method that im unaware of.

Happy with the result
