# Analyzing 10Gb of Yelp Data

In this project, I analyzed a subset of the Yelp's business, reviews and user data to answer the following three questions:

* Do Yelp Reviews Skew Negative?
* Should the Elite be Trusted?
* What is the Most Recommended Restaurant?

The three datasets used in this project originally come from [Kaggle](https://www.kaggle.com/yelp-dataset/yelp-dataset) and they have been uploaded into an S3 bucket: 

* s3://yelpreviewdataset/yelp_academic_dataset_business.json
* s3://yelpreviewdataset/yelp_academic_dataset_review.json
* s3://yelpreviewdataset/yelp_academic_dataset_user.json

## Notebook Configuration
![notebook_configuration](https://user-images.githubusercontent.com/52837649/100153582-4e50ce00-2e72-11eb-9ab0-6a7142c9ee81.png)

## Cluster Configuration
![cluster_configuration](https://user-images.githubusercontent.com/52837649/100153642-60327100-2e72-11eb-8266-f10fc99878c9.png)

## Part I: Installation and Initial Setup
Import the necessary dependencies and load datasets as a pyspark dataframe

## Part II:  Analyzing Categories
Denormalize the categories that are associated with each business and then running some basic analysis on the result

## Part III: Do Yelp Reviews Skew Negative?
To answer the question: are the written reviews generally more pessimistic or more optimistic as compared to the overall business rating. 
<p align="center">
<img width="456" alt="Capture01" src="https://user-images.githubusercontent.com/52837649/100476479-ae828280-30b3-11eb-995a-1094c9a95729.PNG">
</p>

## Part IV: Should the Elite be Trusted? 
<p align="center">
<img width="448" alt="Capture02" src="https://user-images.githubusercontent.com/52837649/100476549-d96cd680-30b3-11eb-8865-5405afb15995.PNG">
</p>

## Part V: What is the Most Recommended Restaurant?
<p align="center">
<img width="601" alt="Capture03" src="https://user-images.githubusercontent.com/52837649/100483635-a3395200-30c7-11eb-9502-0234714903b5.PNG">
</p>

