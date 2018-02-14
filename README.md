# Amazon Fake Reviews Detection
## Codes and dataset developed for MSING055 final project
We collected multiple layers of Amazon product and review data to identify fake reviews. In this repo you will find two notebooks that contain Python code to do:
1. Cleaning data: the data we get from scraping aren't yet ready to be analyzed, so we did several data cleaning steps
2. Generate new attributes for analysis: two new attributes, sentiment_score and most_rev, were generated to do more in-depth analysis

In addition, in `final_dataset` folder, you will find the cleaned final dataset for further analysis.


## Project Summary
The purpose of this project is to contribute to the growing body of studies on identification of fake reviews and reviewers by performing a social network analysis in conjunction with anomaly detection with clustering techniques. Developed approach to identify fake reviewers, relative to previously proposed methods has high potential for automation and enables to significantly narrow down the field and dynamically track fake reviewers. 

The sections below will briefly explain the steps and results.

### Data Collection
We scraped data from Amazon's website by adopting snowball method. The process is summarized in the figure below.
![Alt text](https://photos-5.dropbox.com/t/2/AACPJ1nxubg8w9-CmXK5CMzuK3hsS0f_pS2Ui0sOesL8Ig/12/72523552/png/32x32/3/1518584400/0/2/Screen%20Shot%202018-02-13%20at%2011.58.53%20PM.png/ENK0pzgYy80BIAcoBw/0bsVOBBPanWar3Fu-c4WG5fgga7fNUniiB5ltdTEKqE?dl=0&preserve_transparency=1&size=800x600&size_mode=3)

Two additional attributes were generated through data aggregation and python module:
- most_rev: highest number of reviews made in a day. This help us to identify if user made a very high number of reviews in the same day. 
- review_sentiment: the sentiment score of the reviews, ranging from -1 (negative) to 1 (positive). This help us identify if the rating is inconsistent with the review sentiment. Textblob module was used to get sentiment score.

#### Overview of final dataset
Total rows: 7652
To download dataset: https://github.com/dindatisi/MSING055_Amazon_Review/
Columns:
- url: Scraped url
- review_bold: Title of the review
- ratings: star rating given by the review
- verified: 1 = reviewer is a verified buyer, 0 = if not
- date: date review was made
- by: name associated with profile 
- profile_id: ID number of profile 
- most_rev: maximum review made in a day by profile
- by_link: url to profile
- helpful: number of people who tagged review as ‘helpful’
- product: product name
- product_link: url to product page
- review_sentiment: sentiment score of the review

### Network Construction
Find the code [here](https://github.com/Maytudboon/MSING055)

The networks below show an observable difference between network of suspected fake reviews (left) and normal reviews (right).The nodes are profiles and they are connected to each other if they have reviewed more than 2 products in common. The product on the left was chosen through manual inspection to be suspicious compared to similar products.
![Alt text](https://photos-2.dropbox.com/t/2/AAAs7dSeXtuO4jLQDIUst4myD2q3qZvlWVwCsAse_7OOQQ/12/72523552/png/32x32/3/1518584400/0/2/Screen%20Shot%202018-02-13%20at%2011.57.29%20PM.png/ENK0pzgYy80BIAcoBw/lGZYC35bw8EGxT4Xti-g5viyUvQl7rFMzhHPqH2_Dgs?dl=0&preserve_transparency=1&size=800x600&size_mode=3)

### Clustering
The dataset that has been collected is unlabeled. Therefore we must do through the unsupervised learning path. While the network visualization is used to identify sets of suspicious reviewers, clustering is useful to understand the common characteristics of fake reviews and detect anomaly. The model divided the reviews into five clusters using simple K-Means method with 5 clusters. The attributes included on the models are ratings, verified, most_rev, helpful, and review_sentiment. The cluster model was developed using Weka. 

The charasteristics of each cluster is quite distinguishable, as described below.
![Alt text](https://photos-3.dropbox.com/t/2/AAAHmd2c_cwZ7H05L_NHu9OWOPCZdd6IXXBoql0SjWd6yw/12/72523552/png/32x32/3/1518584400/0/2/Screen%20Shot%202018-02-13%20at%2011.57.16%20PM.png/ENK0pzgYy80BIAcoBw/uI_t134_74GTbER2sBdRXuI3SFRr3_CgrdFySm1KFBw?dl=0&preserve_transparency=1&size=800x600&size_mode=3)

### Matching The Results From Network Analysis and Clustering
Next, we map the suspect list and their reviews into the clusters. We analyzed the review characterstics of our suspect list. Based on the characteristics, we cannot fit their reviews into one cluster. However, 77.35% of the reviews made by these users belong to Cluster1 and Cluster3, as can be seen on the chart below.
![Alt text](https://photos-5.dropbox.com/t/2/AAA0k1ARSJBneY0Yk0MNQFNOO7uhSFIvb5_x5pa-go5Uiw/12/72523552/png/32x32/3/1518584400/0/2/Screen%20Shot%202018-02-13%20at%2011.57.03%20PM.png/ENK0pzgYy80BIAcoBw/jFT9TFYz3TrYWy-TGDyv_OypRigLJgBZFUFNfd3RQF4?dl=0&preserve_transparency=1&size=800x600&size_mode=3)





