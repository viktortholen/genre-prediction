## Hello world!

about the dataset

multilabel prediction

transform to binary with multilabelbinarizer

split data to train and test

pipeline



## Removing things from the plot description!

preprocessing

before

![Word Cloud](/images/wordcloud_without_pre.png){: .center-image}

stopwords

tolower

remove everything that isnt characters between a-z

stemming



after

![Word Cloud](/images/wordcloud.png){: .center-image}

removed genres with low samples under 3000

somewhat scewed data as some genres are represented more

the distribution looks like:

![Word Cloud](/images/genres_graph.png){: .center-image_b}

## Deciding what model to use!

There are many models to choose from

not all models are suited for multilabel

did some tests to see how they compare

### Prediction Distributions Comparisons
![Word Cloud](/images/logisticreg.png){: .center-image_b}

logreg had an okay distribution when comparing the number of predicted genres compared to the real values. 

![Word Cloud](/images/LabelPower.png){: .center-image_b}

labelpower was worse at predicting small sample genres but was pretty similar to logreg

![Word Cloud](/images/BinaryRel.png){: .center-image_b}

binaryrelevance had a really wierd way of predicting as it chose too many genres

when looking at how many genres per label, this one was guessing up to 12 (all genres available) genres per movie

compare to the other label lengths


### Scores
![Word Cloud](/images/scores.png){: .center-image_b} 

need new graph with old score

Using the gridsearch for all of the methods finally resulted in the scores in the graph above.

The scores were in favor of logreg and labelpower as they had almost identical results, however, labelpower was really slow compared to logreg so I continued with improving logreg


## Improving the scores even further

The prediction for a movie is represented by an array of scores between 0-1 for all the 12 genres. 1 represent a genre that true and 0 false. The threshold I chose was 0.5 where scores above that value will be considered as 1 and under 0. 

Many movies ended up with no predicted genres at all. So I tried to get these movies to have at least one genre to make it not feel unrepresented in the genre community... A simple way of doing this was to just change the greatest value in the array to one. This resulted in the score of ???? which is a decent amount with little effort.






###Which one was best?

Logreg was best, labelpower 




