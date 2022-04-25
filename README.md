# Amazon Vine Analysis

## Overview

The purpose of this analysis is to analyze Amazon Vine reviews for **Sports** products dataset, and use PySpark to perform the ETL process to extract the dataset, transform the data, connect to an AWS RDS instance, and load the transformed data into pgAdmin. 

Next, we determine if there is any bias toward favorable reviews from Vine members in this specific dataset. Vine members are paid reviewers.

### Resources

- Data source: Amazon Sports goods dataset from 'https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Sports_v1_00.tsv.gz'.

- Software use to perform the analysis: Google Colab (https://colab.research.google.com/) with PySpark v3.0.3


## Results

### Fetching

- First we obtained the file from Amazon S3 servers regarding the reviews for sports goods and get into a Dataframe.

![Amazon Sports Reviews](/Resources/sports_file.png)


- We had to cast the ***"star_rating*** column to integer because it was read as string.


### Filtering

Applied the following filters in order:

1. Filtered by those reviews that had more than 20 votes.

![Reviews with total votes over 20](/Resources/total_votes.png)

2. Filtered for ratio of helpful reviews to total votes over 50%.

![Reviews with ratio over 50%](/Resources/helpful_votes_filtered.png)

3. Filtered for Paid and Unpaid reviews.

|Review type|Table                                            |
|:----------|:-----------------------------------------------:|
|Paid       |![Paid reviews](/Resources/paid_filtered.png)    |
|Unpaid     |![Unpaid reviews](/Resources/unpaid_filtered.png)|

### Final analysis results

- Using PySpark we calculated the total reviews, five-star reviews and the percentage of five-star reviews:

|Review type|Results                                         |
|:----------|:----------------------------------------------:|
|Paid       |![Paid reviews](/Resources/paid_results.png)    |
|Unpaid     |![Unpaid reviews](/Resources/unpaid_results.png)|


- As shown, after applying all the filters, we have a total of 334 paid reviews versus 61,614 unpaid reviews of sports articles.

- There were 139 Vine reviews with 5 stars and 32,665 non-Vine reviews of 5 stars.

- The latter represents 41.62% of Vine reviews with 5 stars and 53.02% of non-Vine reviews with 5 stars.


## Summary

Apparently, Vine reviews yield a lower percentage of 5-star reviews than non-Vine reviews. With this results we would conclude that there is no positive bias for Vine reviews.

However, it should be noted that only 334 paid reviews were left after applying the filters. This represents only **1.37%** of total Vine reviews for Sports goods, which may not be a representative sample. The same applies to non-Vine reviews which resulted in 61,614 filtered reviews, out of 4,839,483 or **1.27%**.

So taking all the dataset without filtering for total_votes or the ratio of helpful_votes to total votes we get the following results for each rating:

|Vine Reviews                                    |Non-Vine Reviews                                        |
|:----------------------------------------------:|:------------------------------------------------------:|
|![All Vine reviews](/Resources/vine_ratings.png)|![All non-Vine reviews](/Resources/non_vine_ratings.png)|

From these numbers we calculate the percentage of 5-star reviews for Vine reviews at 44.45% and 62.73% for non-Vine reviews. However, if we analyze 4-star reviews, the percentage is higher for Vine reviews than for non-Vine ones, 37.3% vs 17.2%, respectively. Adding both 4 and 5-star reviews give us 81.8% for Vine reviews and 79.94% for non-Vine reviews.
Thus, the conclusion is not clear as whether there is no positive bias for Vine reviews in Sports articles.

### Recommendations

In order to enhance the analysis we recommend the following:

- Analyze through a t-test both rating means to see if there is a statistical difference among paid and unpaid reviews.
- Make a regression analysis to determine outcome probability of a determined rating based upon: paid/unpaid review, verified purchase and helpful reviews.
- Compare Vine Program results for other goods to see if the conclusions are the same.