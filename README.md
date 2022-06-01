# YouTube Censorship in Culture and Politics

## Overview

YouTube is the most popular video platform online. In the United States, YouTube is a significant source for news. With such a wide assortment of community created content, moderating YouTube content is a herculean task. Given the complex considerations for censoring political content, and sheer amount of videos, YouTube uses blackbox Machine Learning models to remove objectionable content.

We aim to understand what factors influence the removal of political videos on Youtube, and to what degree can we predict whether a video will be taken down. We further attempt to predict the time YouTube took to remove these videos.

  ● RQ1: How accurately can we predict which political videos will be removed by YouTube's algorithm using video metadata?  
  ● RQ2: How accurately can we predict how long it takes for a political video to be removed based on its metadata?  
  
## Data description

Our data mainly comes from two sources: Transparency Tube and Youtube Data Tools. 

Transparency Tube categorizes and analyzes over 7,300 of the largest English language YouTube channels actively discussing political and cultural issues, scraping these channels semi-regularly. When videos are no longer available, they are added to a dataset of removed videos.  

![image](https://user-images.githubusercontent.com/37094544/171314039-e4c4da5a-61b8-4a7a-83b9-601fa29ba26a.png)

For our prediction task, however, we benefit from assessing videos that are and aren’t removed. Which is why we randomly selected three channels for each of the 18 political categories  to retrieve using Youtube Data Tools, as we consider political affiliation of a channel to be an interesting factor for removal.

### Feature Engineering

Our dataset contains a variety of variable-types ranging from numeric, categorical, textual to binary. While numeric, categorical and binary variables are easy to incorporate in modeling, textual data needs some encoding. Therefore, we use BERTBASE (Bidirectional Encoder Representations from Transformers), a transformer based machine learning technique for NLP.6 which essentially converts textual data into a variable number of encoder layers.

## Data preprocessing

For RQ1, data cleaning to create our working dataset is detailed in the data cleaning file. Beginning from this working dataset, we needed to create samples for analysis. The removals we are interested in predicting are rare: about 4% of 35,463 observations. We therefore sampled in two ways: with removed to unremoved in proportion to the actual dataset, and with removed to unremoved in even proportion to allow our models to perform better. For the latter, given that we have 1,425 removed observations, we randomly selected 1,425 unremoved observations, for a total n=2,850. For each of these, we sampled 10%, 50%, and 100% of the dataset. For each sample, we used an 80-20 train-test split.

![image](https://user-images.githubusercontent.com/37094544/171313972-ca82e848-09e5-4c13-9668-b7d444cdb25d.png)

For RQ2, the dataset only contains the removed videos. Sampling was not necessary since there was no concern over any imbalance within the dataset.

## Implementation

For RQ1, we run a series of models roughly going from least to most complicated: logistic regression, random forest, decision tree, SVM, and neural network. Each model was run six times: for the data with removed and unremoved videos in equal proportions, at 10%, 50%, and 100% sample size; for the data with removed and unremoved videos proportional to the real world data, at 10%, 50%, and 100% sample size. With six runs each of five different model types, a total of 30 models were attempted. However, due to computational restraints, only 26 ran successfully on R.

For RQ2, we ran a total of three Linear Regression models to assess the impact each predictor had towards making an accurate prediction. For model 1, we made use of only video metadata that was publicly available to make predictions; model 2 included the associated political leaning of the video as an additional predictor; lastly, model 3 included the channel_id as an additional predictor.

## Conclusion

We have found that we can predict if a political video will be removed with reasonable accuracy (~87%) with a relatively simple decision tree. However, the models tend to fail if we use a representative dataset, where only around 4% of videos are removed. In these instances, precision and recall are very poor.

In terms of explainability, the political leaning and channel videos come from having significant explanatory power. This makes intuitive sense, and also suggests that there is reasonable concern that YouTube’s algorithm is biased towards certain political leanings. For example, the decision tree shows that right-leaning political videos are indicative of higher removal likelihood (RQ1), while the regression (RQ2) shows that right-leaning political videos are taken down quicker.
