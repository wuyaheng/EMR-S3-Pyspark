# Analyzing 10Gb of Yelp Data

In this project, I analyzed a subset of Yelp's business, reviews, and user data to answer the following three questions:

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
```
from pyspark.sql import SparkSession
my_spark = SparkSession.builder.getOrCreate()
sc.install_pypi_package("pandas==1.0.3")
sc.install_pypi_package("matplotlib==3.2.1")
sc.install_pypi_package("seaborn==0.10.0")
sc.install_pypi_package("wordcloud==1.8.1")
sc.list_packages()
```

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
import pyspark.sql.functions as F
from pyspark.sql.functions import explode, split, desc, col, avg, udf, when
```

```
business_df = spark.read.json('s3://yelpreviewdataset/yelp_academic_dataset_business.json')
review_df = spark.read.json('s3://yelpreviewdataset/yelp_academic_dataset_review.json')
user_df = spark.read.json('s3://yelpreviewdataset/yelp_academic_dataset_user.json')
```

## Part II:  Analyzing Categories
Denormalize the categories that are associated with each business and then running some basic analysis on the result
We need to "break out" these categories from the business ids? One common approach to take is to build an association table mapping a single business id multiple times to each distinct category.

For instance, given the following:

| business_id	| categories |
|:-----------:|:----------:|
| abcd123	| a,b,c |

We would like to derive something like:

| business_id	| category |
|:-----------:|:--------:|
| abcd123	| a |
| abcd123	| b |
| abcd123	| c |


## Part III: Do Yelp Reviews Skew Negative?
To answer the question: are the written reviews generally more pessimistic or more optimistic as compared to the overall business rating. 
<p align="center">
<img width="456" alt="Capture01" src="https://user-images.githubusercontent.com/52837649/100476479-ae828280-30b3-11eb-995a-1094c9a95729.PNG">
</p>
The distribution of skew appears to be normal, but skewed a little bit to the right. The implications of the above graph are that the satisfaction level of reviewers who left positively skewed reviews is greater than the dissatisfaction level of reviewers who left negatively skewed reviews. In other words, reviewers who left a written response were more satisfied than normal.

## Part IV: Should the Elite be Trusted? 
<p align="center">
<img width="448" alt="Capture02" src="https://user-images.githubusercontent.com/52837649/100476549-d96cd680-30b3-11eb-8865-5405afb15995.PNG">
</p>
As we can see from the above boxplot, elite data has more outliers. Additionally, the first, third quantiles and the median of the elite ratings are also higher than the non-elites' ratings. From my point of view, I would say elite should not be trusted because they tend to give higher ratings compared with the ratings given by non-elite.

## Part V: What is the Most Recommended Restaurant?
1. First of all, I used business dataset to filter down to restaurants with five star rating only. Next, I grouped by city to see which city has the most five star rated restaurants. From the plot below, we can tell that Las Vegas has the largest number of 5-star rated restaurants. 
<p align="center">
<img width="601" alt="Capture03" src="https://user-images.githubusercontent.com/52837649/100483635-a3395200-30c7-11eb-9502-0234714903b5.PNG">
</p>

2. Second, I found out that Water Grill has the largest number of reviews left by Yelpers among so many 5-star rated restaurants at Las Vegas. Based on the findings from Part III, reviewers who left a written response were more satisfied than normal, we can conclude that Water Grill must be worth checking out.
<p align="center">
<img width="604" alt="Capture04" src="https://user-images.githubusercontent.com/52837649/100483714-dda2ef00-30c7-11eb-9e7b-35b1333617fa.PNG">
</p>

3. Lastly, I created a WordCloud for the written reviews of the Water Grill, a restaurant that has more than 1750 reviews.
<p align="center">
<img width="430" alt="Capture05" src="https://user-images.githubusercontent.com/52837649/100483869-5609b000-30c8-11eb-8536-b56e888f0794.PNG">
</p>

