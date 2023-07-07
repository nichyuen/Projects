# Mental Health for Today's Youth
**Problem Statement**<br>
MOH will want to intstill preventive measures such as hate speech or online bullying detection on social platforms that youths frequent (eg. Tiktok, Youtube, Twitch). Thus the purpose of this project is on creating a model to effectively filter text comments with elements of hateful sentiments on such platforms. 

As an analyst in MOH, the task at hand to have an effective classifying model to process text comments 

- Acceptance performance metric:
  - At least 0.75 for Recall, since the main objective is to effectively filter hateful comments (i.e Minimise false negatives).
  - At least 0.65 for Precision, less emphasis on minimising false positives but still important to a degree to ensure freedom of speech is maintained.

**Notebooks**<br>

00_DataCollection<br>
01_Cleaning-EDA<br>
02_Modeling<br>


**README Overview**<br>
1. Data<br>
2. Approach<br>
3. Models Performance<br>
4. Conclusion<br>

# 1. Data:
The data used in this project is sourced from using Google's [Youtube API](https://developers.google.com/youtube/v3). The comments extracted are from selected videos which are popular among Youth (eg. Logan Paul, MrBeast) to have a more relevant analysis to the problem at hand. The modelling process will be carried out with the assistance from google's [Perspective](https://developers.perspectiveapi.com/s/about-the-api?language=en_US) in the labeling of about 35,000 comments.

Summary of dataframe that is worked on:
|Feature/Column|Description|
|---|---|
|text_translated|Youtube videos' comments that are translated to english|---|---|
|likeCount|Amount of likes given by viewers of the comment|---|---|
|TOXICITY|Attribute score (range from 0 to 1) on the level of Toxicity of the comment by Perspective (weighted at 0.4). Higher score implies higher level of toxicity|---|---|
|INSULT|Attribute score (range from 0 to 1) of the comment being insulting by Perspective(weighted at 0.25). Higher score implies comment contains more insulting context|---|---|
|IDENTITY_ATTACK|Attribute score (range from 0 to 1) of the comment targeting specific groups of individuals by Perspective(weighted at 0.25). Higher score implies comment have words that discriminate groups of individuals|---|---|
|THREAT|Attribute score (range from 0 to 1) of the comment posing as threats in hurting individual's life by Perspective(weighted at 0.1). Higher score implies comment contains threatful context|---|---|
|weight_mean|Weighted average of the attribute scores. Higher score implies comment contains higher degree of hateful context|---|---|
|hateful|Classification on whether the comment is contains hateful context (0: Non-hateful, 1: Hateful)|---|---|<br>


# 2. Approach:

|Approach|Description|
|:---|:---|
|Data size|9 columns, 61,646 rows|
|Data Cleaning & Processing|<ul><li>Overall duplications check</li><li> Individual feature data check</li><li>Drop redundant and irrelevant features</li><li>Checking for null values</li><li>Removal of HTML tags<li>Translation of comments to english<li>Labeling with Google's Perspective through aggregating  weighted mean<li>Dataframe has 34,484 rows  after cleaning|
|EDA|<ul><li>Distribution of data balance</li><li>Distribution of attribute scores</li><li>Distribution between amount of likes and attributes of comments</li><li>Spread between each attributes</li><li>Common words in exhibiting the types of attributes|
|Modelling|<ul><li>Preprocessing<ul><li>Stemming of tokens</li><li>TF-IDF vectorize</li><li>Oversampling of minority data</li></ul><li>Approach 1A: Based model <ul><li>Use of Naive Bayes as benchmark model to have base understanding on metrics</li></ul><li>Approach 1B: Oversample minority on based model</li><ul><li>Using same set of hyperparameters with oversampled minority(hateful comments) data using RandomOversampler</li></ul><li>Approach 2A: Attempts to reduce Overfitting</li><ul><li>Use of ridge regularization from Logistic Regression</li></ul><li>Approach 2A*: Tweaking decision threshold </li><ul><li>To maximise Recall through tradeoff of Precision</li></ul><li>Approach 3: 2nd attempt to reduce Overfitting </li><ul><li>Use of Random Forest classifier</li></ul>|

# 3. Models Performance:

**Overview of Models performance:**


|Approach|Description on best parameters|Metrics|Conclusion|
|-|-|-|-|
|1A|TF-IDF Vectorization<ul><li>english stopwords filter</li><li>max document frequency:90%</li><li>min document frequency:3times</li><li>uni/bi-gram</li></ul><br>Naive Bayes classification|Recall<ul><li>0.1684(test)</li><li>0.1815(train)</li></ul><br>Precision<ul><li>0.8876(test)<li>0.8947(train)</li></ul>|Poor metrics likely due to imbalanced data|
|1B|TF-IDF Vectorization<ul><li>english stopwords filter</li><li>max document frequency:90%</li><li>min document frequency:2times</li><li>uni/bi-gram</li></ul>Naive Bayes classification<br><br>Oversampler<ul><li>minority data(hateful comments)</li></ul>|Recall<ul><li>0.6631(test)</li><li>0.9942(train)</li></ul><br>Precision<ul><li>0.3459(test)</li><li>0.9480(train)</li></ul>AUC-PR: 0.5544|Overfitting model with low scores|
|2A|TF-IDF Vectorization<ul><li>english stopwords filter</li><li>max document frequency:90%</li><li>min document frequency:2times</li><li>uni/bi-gram</li><li>max features: 10000</li></ul><br>Logistic Regression<ul><li>C:1</li><li>solver:'liblinear'</li></ul><br><br>Oversampler<li>minority data(hateful comments)|Recall<ul><li>0.7719(test)<li>0.9931(train)</li></ul><br>Precision<ul><li>0.7154(test)<li>0.9840(train)</li></ul><br>AUC-PR: 0.7339|Overfitting model with improved scores|
|2A*|Same parameters as 2A with adjusment to decision threshold to 0.4104|Recall<ul><li>0.8124(test)</li></ul><br>Precision<ul><li>0.6524(test)</li></ul>|Overfitting model with further improvment to Recall with the tradeoff of Precision|
|3|TF-IDF Vectorization<li>english stopwords filter<li>max document frequency:90%<li>min document frequency:2times<li>uni-gram<li>max features: 10000</li><br>Random Forest classifier<li>C:1<li>solver:'liblinear'<br>Oversampler<li>minority data(hateful comments)|Recall<ul><li>0.8210(test with decision threshold adjustment)<li>0.8420(train)</ul><br>Precision<ul><li>0.6332(test with decision threshold adjustment)<li>0.9690(train)</li></ul><br>AUC-PR: 0.7293|Improved scores on Recall and overfitting has significantly reduced in addition with adjustment of decision threshold to 0.49|


**Best Performing Model: Approach 3**<br>
    
This model is selected for the classifier given the metric in focus (Recall) have been satisfied (>0.75 Recall score), its secondary objective (>0.6 Precision score) was slightly underacheived implying that the model will likely have more cases of False positives than False negatives. 

It is also noted that there is overfitting based on the Precision metric (test: 0.596 & train: 0.964) which implies that the model aggressive in classifying comments to be hateful.

However, the classification model is still useful to effectively identify potential hateful comments which can act as a primary filter for the user before a second in-person check can take place which saves time and resources for the user. For comments that are found to have non-hateful context will be approve for publishing. 


# 4. Concluding statements:<a class="anchor" id="ID4"></a>

**Furture work**
- Gather more balanced data (i.e more hateful comments) instead of using Oversampler to balance data since the model will be trained on similar tokens that posed hateful context which might not be reliable in classifying unseen data.

- Gather wider range of context of videos for better performance on general comments. Since the current model is trained on videos that are more popular among the younger generation, the slangs/lingo use by the population can be different thus affecting the classification results on a comment from a different demographic.

- Removal of the common tokens that appear the most in hateful/non-hateful comments to reduce ambiguity for the model in classifying. This would likely to improve both Precision and Recall scores as it minimizes both False positives and False negatives which will acheive our initial goal of having an auto-classifier without the need of manual filtering.

- The project is targed to detect hateful comments based on the determined weightage of attributes where it emphasises on Toxicity and less on other attributes. This can be adjusted according to the user's objective.
    - For example, to detect for possible suicidal/troubled youths or radicalized potential terrorists by adjusting higher weightage on the Threat attribute. To detect for individuals that attempts to disrupt racial harmony through racist remarks, the user can adjust higher weightage on the Identity_attack attribute.
