# Module 16 : Amazon Vine Analysis 

## Project Overview 
### Purpose of Module 15 
In this module, we were introduced to cloud services and natural language processing in efforts to explore big data and describe its challenges. We were introduced to Apache Hadoop and with it, explored the MapReduce paradigm to process big data. We then compared this method of processing data to Apache Spark and explored its architecture and use of clusters. We explored the vast uses of natural language processing and the types of analyses that can be performed and practiced NLP pipelines to transform datasets. We began to use RDS and S3 with Amazon Web Services and used AWS and Spark to transform raw data and write directly to a PostgreSQL database. 
 
### Overview of Assignment 
For this assignment, we were challenged to complete an analysis of product reviews for a marketing company. Through natural language processing, we aimed in the module, to explore translating words to numbers to analyze reviews. In the challenge, we aimed to determine if there is a bias towards favorable reviews from a dataset. In essence, we used PySpark to perform an ETL process to extract a dataset from AWS S3, transform the data, and connect the data to an AWS RDS instance and load it into pgAdmin. 

### Resources 
    VS Code Version: 1.63.2
    Google Colab 
    Amazon Web Services 
    pgAdmin Version 6.1
    SQL 
    Apache Spark Version 3.0.3 
    SQL Schema: challenge_schema.sql 
   [AWS S3 dataset](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt)
   
   AWS S3 chosen dataset [Kitchen Products](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Kitchen_v1_00.tsv.gz) 


## Results 
### Deliverable 1 
In deliverable 1, we performed the ETL process in efforts to create an AWS RDS database with tables in pgAdmin. After creating our spark session, we loaded the AWS S3 data into a spark dataframe as shown in the code below: 

<img width="1141" alt="sparkdf" src="https://user-images.githubusercontent.com/92558842/153975962-608554d5-cb3a-4d8b-8c30-e1de1d52554d.png">

We then began to transform the dataframe into four separate data frames that matched the given schema in pgAdmin. We created the *customers_df* that contained that grouped the customer_id column to create the corresponding customer_count column.  

<img width="1125" alt="customers_df" src="https://user-images.githubusercontent.com/92558842/153975981-fce4bdba-a121-42cc-acbb-cef32c549206.png">

The next dataframe created was the *proudcts_df* which contained unique product IDs as the product_id column and product_title column. 

<img width="686" alt="products_df" src="https://user-images.githubusercontent.com/92558842/153976035-9b5e41e7-384c-4ce3-b44f-1940db942e41.png">

Next, we created *review_id_df* that contained the review_id, customer_id, product_id, product_parent, and review_date columns. We further transformed this dataframe by converting the review_datae column to a date using to_date() function. 

<img width="1221" alt="review_id" src="https://user-images.githubusercontent.com/92558842/153976052-851e333e-280f-4b93-ac41-f6b26aef6979.png">

Lastly, we created *vine_df*  that contained the review_id, star_rating, helpful_votes, total_votes, vine, and verified_purchase columns. 

<img width="939" alt="vine_df" src="https://user-images.githubusercontent.com/92558842/153976065-57a6cc0c-ce62-4b0f-9987-58a4d6d73680.png">

We then connected our transformed data to the AWS RDS instance and wrote each dataframe to its corresponding table in pgAdmin. To ensure our data was properly populated in pgAdmin, we ran queries to display each table and the transformed data. The following images display the tables:
#### customer_table:
<img width="266" alt="customers_table" src="https://user-images.githubusercontent.com/92558842/153976272-f6a54b89-58e0-46c5-9ec1-7a66e11a2aa2.png">

#### products_table: 
<img width="638" alt="products_table" src="https://user-images.githubusercontent.com/92558842/153976303-eff2c49d-9d63-4ab3-b505-e6a6d96209d8.png">

#### review_id_table: 
<img width="594" alt="review_id_table" src="https://user-images.githubusercontent.com/92558842/153976292-da45a29c-58dc-4b0f-b9f8-0ad114fb05eb.png">

#### vine_table: 
<img width="617" alt="vine_table" src="https://user-images.githubusercontent.com/92558842/153976253-9892db26-f03e-460f-86d8-095167c0f6b0.png">


### Deliverable 2 
To complete deliverable 2, I used PySpark and Google Colab to recreate the vine_df from the AWS S3 dataset to further analyze the reviews. I initially filtered the vines dataframe to retrieve all rows where the total_votes count is greater than or equal to 20 in order to pick reviews that were more likely to be helpful. 

<img width="719" alt="filtered_df" src="https://user-images.githubusercontent.com/92558842/153976396-461578c4-350b-44f1-ba19-556fd1eefef9.png">

We further filtered this dataframe by retrieving all the rows where the number of helpful_votes divided by total_votres is equal to or greater than 50%. 

<img width="800" alt="top50" src="https://user-images.githubusercontent.com/92558842/153976421-40f07166-1d2f-48e8-b77e-76b3b8dfaf26.png">

To aid our analysis in determining if having a paid vine review makes a difference in the percentage of 5-star reviews, I filtered the *top50_df* for rows where a review was written as part of the vine program (paid/‘Y’) and where the review was not part of the vine program (unpaid/’N’).
#### Paid/ Part of the Vine Program: 
<img width="801" alt="paid" src="https://user-images.githubusercontent.com/92558842/153976459-75aa516e-770e-4b7c-8682-67d955d9d281.png">

#### Unpaid/ Not Part of the Vine Program: 
<img width="834" alt="unpaid" src="https://user-images.githubusercontent.com/92558842/153976463-41b71ced-8560-46d8-9c98-4354e7b1f87e.png">

I also created a five-star reviews dataframe that contained all of the instances where the star_rating was equal to five. 

<img width="726" alt="fivestar" src="https://user-images.githubusercontent.com/92558842/153976512-eaa0087c-72a8-4e83-965d-8e85f1703a68.png">

From the dataframes created, we were able to determine the following: 

- How many total reviews were in the filtered dataframe?  
  For the Kitchen Product reviews collected from AWS S3 that we transformed, there are 99046 total reviews. 
 
- How many five-star reviews were there?  
  From the five-star reviews dataframe created, we find for the dataset, that there were 46367 five-star reviews. 
  
- What is the percentage of five-star reviews?  
  We find that for our dataset, 46.81% of the reviews have a five-star rating. 
  
- How many vine reviews and non-vine reviews were there?  
  According to the transformed dataset, there were 1207 paid vine reviews and 97839 unpaid vine reviews. 
  
- How many Vine reviews were 5 stars?   
  By filtering the *paid_vine* dataframe for star_rating equal to 5, we find that there are 509 total five-star paid vine reviews. 
  
- How many non-vine reviews were 5 stars? 
  By filtering the *unpaid_vine* dataframe for star_rating equal to 5, we find that there are 45858 total five-star unpaid vine reviews. 
  
- What percentage of vine reviews were 5 stars?   
  We find that 42.17% of paid vine reviews were five star. The following image shows the data used to collect the values pertaining to paid vine reviews. 

  <img width="719" alt="paidvinereviews" src="https://user-images.githubusercontent.com/92558842/153976884-1708cf18-8b13-421d-8fb4-fbad0df6cded.png">

- What percentage of non-vine reviews were 5 stars?   
  We find that 46.87% of unpaid vine reviews were five-star. The following image shows the data used to collect the values pertaining to unpaid vine reviews. 
  
  <img width="774" alt="unpaidvinereviews" src="https://user-images.githubusercontent.com/92558842/153976912-f33a4db5-ad91-4238-99dd-34ce5909cf07.png">

## Summary 
Overall, with a 4.7% difference between unpaid and paid vine reviews, I am not confident in saying that there is any bias for reviews in the vine program. However, since the percentage of unpaid vine five-star reviews was higher than that of paid reviews, one could suggest that there is slight bias towards unpaid vine reviews; however, the analysis should be repeated since the sample size for paid vine reviews is significantly less than that of unpaid vine reviews (there are 96000 more unpaid vine reviews). 

### Additional Analysis 
To further analyze the dataset, one could utilize the review_body column and natural language processing to translate the key words to numbers and analyze the general positivity or negativity towards a product. One could also group they data by the product_category column and compare the associated reviews to determine which product category tends to have positive or negative reviews. It may also be beneficial to redo the challenge analysis where the dataset is filtered to where the review is on a verified purchase (verified_purchase column filtered to ‘Y’) to pick more accurate reviews. Furthermore, to determine if there is bias in the reviews for the vine program, one could repeat the analysis with multiple datasets and perform an ANOVA test to determine if there is a statistical difference between the mean percentage of paid vine five-star reviews and unpaid five-star reviews for the different datasets. 
