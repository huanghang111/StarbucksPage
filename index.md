layout: page
title: "blog page"
permalink: /blogPage/

@[TOC](Starbucks Offers Study)

# Udacity Data Scientist Nano Degree - Capstone Project
![Starbucks Offer Study](https://img-blog.csdnimg.cn/20200729171851382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)

## Project Introduction
This data set contains simulated data that mimics customer behavior on the Starbucks rewarding system in their mobile application. Once every few days, Starbucks sends out an offer to users of the mobile app. The message can be an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks. We are going to analyze three file:
1. portfolio: containing offer ids and meta data about each offer (duration, type, etc.). 10 rows, 6 columns.
 2. profile: demographic data for each customer. 17000 rows, 5 columns.
 3. transcript: records for transactions, offers received, offers viewed, and offers completed. 306534 rows, 4 columns.

The process of our analysis will be by the following step: Define our Business question, understanding the data sets, Data preparation and wrangling, analyze the data, model the data, compare model performance, and finally selecting one model and improving it.

## Business Model
Starbucks is wondering how they can increase the total sales revenue by providing different offers to different customers. That's exactly the target of this project, namely, investigation of the corresponding offers according to customer profiles. The objective of Starbucks is to find out the relationship between customers patterns and offer types.

## Data Model
Now, let's go through the data tables, so as to get an overview of the data sets. The raw dataframes will be illustrated as follows.
1. portfolio dataframe 
![*portfolio* ](https://img-blog.csdnimg.cn/20200729171456110.png)
2. profile dataframe
![*profile*](https://img-blog.csdnimg.cn/20200729171459631.png)
3. transcript dataframe
![*transcript*](https://img-blog.csdnimg.cn/20200729171502175.png)
## Data preprocessing and wrangling

Based on what we have seen in the previous step, there needs to be some work to prepare the data for analysis and modeling.
For our first dataframe which is portfolio, we can see that the ‘channels’ column need some work. Why? because it contains a list, so each value in that list must have its own column. After separating each value, we will obviously need to drop the ‘channels’ column as it is no longer needed. The table will look like this:
![data_preparation_1](https://img-blog.csdnimg.cn/2020072922590030.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
Looks better right! what makes it even better is that we don’t have any NaN values! so on to the next one.
For our next dataframe profile, we had some NaN and None in both ‘gender’ and ‘income’. First, we will replace the None in ‘gender’ with N/A then replace the NaN in ‘income’ with the average of income. After applying these two steps the table will look like this:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729230027499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
It becomes much easier to check the information. Besides, the transcript dataframe contains no NaN, which is confirmed by the statistic output.

```
event		0
person		0
time		0
value		0
dtype:		int64
```
But, as seen above, the ‘value’ column contains a dictionary that means we have to separate each value and drop the ‘value’ column as it is no longer needed. To see what value it holds, we use for-loop and find the keys. After iterating through the ‘value’ column we can find that we have the following keys:[‘offer id’, ‘amount’, ‘offer_id’, ‘reward’]. Our next step is to iterate over transcript table, check value column and update it, put each key in separated column, and finally delete the ‘value’ column. After applying what I’ve discussed, the table will look like this:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729230354840.png)
## Data Analysis Result

### Uni-variate Exploration
- What is the average income for Starbucks customers?
- What is the average age for Starbucks customers?
-  What is the most common promotion?
-  What are the most common age group and gender?
- Who are the most loyal customer (most transcripts)?

We will start with the first question, what is the average income for Starbucks customers? its quite easy to calculate simply use the .mean() and the average income is:

```
65404.99156829799
```
it’s not that low, but put in mind that this is their annual income. On to the next question. What is the average age for Starbucks customers? to do this it is similar to our previous question, use the .mean() function. The output is:

```
62.53141176470588
```
Our third question is, What is the most common promotion? This one was a little bit tricky as they needed to be converted to text first. After converting and encoding/decoding, I decided to show the top 3 promotion only and show only the completed promotions as they are more important. So, we got the following output:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729231044122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
BOGO (buy one get one free) is the most used followed be discount with a small difference. While informational came third with ~40000 difference, that’s a huge gap.
On to the forth questions, what are the most common age group and gender? First, I decided to use age group to make it easier to deal with and read. The following code shows how I divided them:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729231150765.png)
After creating the age_group, we can start answering our question. Let’s look at what age group most customers are:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729231350569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
It shows that the customers mainly consist of adult and elderly groups. Next the gender property is verified:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072923161717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
According to the statistics, males are 2000 more than females. Now let’s look at our final Univariate related question, Who are the most loyal customer (most transcripts)? This might help us so we can give them more promotion to rewards their loyalty. To approach this question, we can order the ‘amount’ in descending order and get the top 10. After applying the action, we will get the following output:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730000422251.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730000520236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
As shown above, we can see their Profile ID as each customer has a unique number, Number of Completed Offers, and the Amount. By this data, we can give them extra and unique promotions in order to reward them. Now that we have completed the univariate part, let’s move move on to next one.

### Multi-variate Exploration
- What is the most common promotion for children, teens, young adult, adult and elderly customers?
- From profiles, which get more income, males or females?
- Which type of promotions each gender likes?

Let’s look at the first question, what is the most common promotion for children, teens, young adult, adult and elderly customers? Since it’s a Multivariate question we’ll use a multi bar chart. The output will be:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730000841750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
We can observe that all of them have similar results in offer type, Transactions has the upper hand, followed by BOGO. We can also see that young adults and teens aren’t our main customer group, so we can focus on elderly and adults.
Our second questions is, from profiles, which get more income, males or females? For this question we will ignore the N/A group because they haven’t specified their gender. Furthermore, we will use a plot called violin and use both income and gender to answer our question. The output will look like:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730001001414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
The graph above shows that income median (the white dot) for females (around 70k) is higher than males (around 60k) we can also see that for females the income spreads from 40k to 100k. For males most of them around 40k to 70k which close to median.
Our third and final question is, which type of promotions each gender likes? This questions is similar to our first question, but our focus now is on gender. So, we’ll use a multi bar chart.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730001049604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5naGFuZzExMQ==,size_16,color_FFFFFF,t_70)
It seems that they all share the same interest and prefer BOGO, but we can’t ignore discount and the difference between them is low.
It looks like we have successfully covered the analysis part. Now, we will focus and machine learning and applying different models.

## Machine Learning Models  
I tried to make a model that can identify which kind of offers we should give a customer. Because my model will guess the offer_type, I will only get those transcripts with offer id’s. So I will ignore all transactions without offer id’s.
Since we have a simple classification problem, I will use accuracy to evaluate my models. We want to see how well our model by seeing the number of correct predictions vs total number of predictions. Why choose accuracy? First let’s define accuracy, the ratio of the correctly labeled subjects to the whole pool of subjects. Also, accuracy answers questions like: How many students did we correctly label out of all the students? It’s similar to our situation right? because we want to see how many customers use Starbucks offers. Furthermore, Accuracy = (TP+TN)/(TP+FP+FN+TN). Not to forget, that this is a simple classification problem, so this is my opinion and reasoning on why to use the easiest (accuracy). Based on the machine learning theory, the features of our model are:
```
Event
Time
Offer_id
Amount
Reward
Age_group
Gender
Income
```
Target is:

```
offer type
```
The model theories involved in the verification process are:
 - [ ] Logistic Regression
 - [ ] K-Nearest Neighbors
 - [ ] Decision Tree
 - [ ] Support Vector Machine
 - [ ] Random Forest
 - [ ] Naive Bayes

## Performance of Different Modelling Theories
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730002153981.png)
Based on the above table, we can see that we’ve scored 100% accuracy in the training and testing datasets on 4 models. To avoid overfitting, I will choose Logistic Regression since it got good results 80.5% on training and 92.8% on testing datasets. Logistic Regression is better used here since we have few binomial outcomes. It's also good here because we have a decent amount of data to work with. 

## Model Improvement
Now, let’s improve our model to have better results. After using Grid Search with Logistic Regression we managed to get better results as shown here:

```
Best Score: 0.8217311874728143
Best params: {'C': 3.5, 'dual': True, 'max_iter': 100}
```
The result show 1.7% improvement, which is nice performance enhancement.

## Project Conclusion
In this project, I tried to analyze and make model to predict the best offer to give a Starbucks customer. First I explored the data and see what I have to change before start the analysis. Then I did some exploratory analysis on the data after cleaning. After that I trained the data, then choose one model and improved it to get better results. In conclusion, I think that Starbucks needs to focus more on adults and Males. Also, offer more BOGO and discounts to their customers.
