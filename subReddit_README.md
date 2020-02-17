# Subreddit Classifier

## Outline

- Data Gathering
- Data Cleaning
- Data EDA and Data Visualization
- Modeling
- Results & Conclusion

### Data Gathering

`This was completed in the Data Capture and Cleaning notebook`

To gather the data, I used Pushshifts API to scrap 500 submissions and repeated this process until I had 10,000 data points for each subreddit. Things to keep in mind were, continuing after the last submission was pulled, as to not repetively pull the same 500 submissions, and add a sleep timer of 30 seconds to ensure that I didn't have my IP banded. 

After all 10,000 submissons were gather for my two subreddits `r/wallstreetbets` and `r/investing`, I appended these two dataframes together and removed any duplicates. I created two csv exports, `df` was a dataframe for the selftext, title, and subreddits, and df_all had all of the columns remaining in case I wanted to pull any other data. During the duration of this project I used `df`.

### Data Cleaning

`This was done in the EDA & Modeling, Data Visualization, and Production Model notebooks`

One key insight I gained while browsing `r/wallstreetbets` was the enormous amount of `[removed]` text in the selftext. In this case, I deciced that I would fill all of the NaN values with `[removed]`. After fitting specifically the selftext and title to seperate models, I decided to combine them together into one column called title and selftext.


### Data EDA and Data Visualization

`This was done in the EDA & Modeling and Data Visualization notebooks`

After the data was cleaned and collected, I looked into each portion a little more, specficially understanding the word counts. By referencing `spooky-eda`, I was able to obtain the most commonly used words in the self text and title. While graphing, I noticed that of all words used, r/wallstreetbets had more `[removed]` text and more text involving calls and tesla. r/investing on the other had more text on investments, stocks, time, and the longevity of investments. I also used vaderSentimentAnalyzer to decern any positive, negative, neutral, and compound differences between the two subreddits. Only the selftext and title & selftext columns had any differences, specifically in the compound catagory. 


### Modeling

`This was done in the EDA & Modeling and Production Model notebooks`

Until I note differenly, I ran both the selftext and title columns as their own seperate models.

I used Logistic Regression, under multiple paramters, with CountVectorizer and TfidfVectorizer; however, I did not used CountVecotizer any further because it did not perform better than TfidfVectorizer. The models I used with TfidfVectorizer any their responese were noted below.

- Logistic Regression: This was my best model and used in the production model
- Gaussian: This was extremely overfit.
- Random Forest & KNN: This took extremely long to run and needed a large amount of adjusting to get a model close to Logisic Regression. `I ultimately removed these in case anyone wanted to run my code`.

    Ultimately, I fit and transformed the column with the title & selftext combined to TfidfVectorizer, and included the compound score in my final Logisic Regression Model. I also took time to find how removing some of the most commonly used words would change my accuracy score, but to great suprise it didn't change the score even after removing all of the top 10 used words. With both the compound score and the combined title & selftext combine, I got an accuracy score of `0.94` on the training set and `0.879` on the testing set.

The best parameters were ridge regression with a C of 2.5. The following table is what I entered into the parameters. 

| Penalty | L1 | L2  |   |     |    |
|---------|----|-----|---|-----|----|
| C       | 1  | 2.5 | 5 | 7.5 | 10 |
 

### Results & Conclusion

I was able to obtain a model with 87.4% accuracy and a ROC_AUC score of 0.94. Both subreddits had a little over 300 sumbmissions misclassified, which could be due to the overlap these two subreddits already had, or it could be from a user posting in both subreddits. Obviously the remove duplicates function I used would remove any posts that were in both subreddits; however, the language for these remaining 600 missclassified posts could be so close to either side that it would be impossible to decern which subreddit it was posted too. I would be interested to see how these postes would be classified if the comments were included, since comments in each subreddit could differ more severly than the submission itself. I would also be interested to see what would happen if I were to remove all of the`[removed]` posts, because it could be usful for a bot that has to classify a post that has the full text remaining. Just to reiterate, many of the wallstreetbets posts have this `[removed]` text because the bots or users removed the selftext becuase it broke the rules. 






