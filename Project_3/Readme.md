### Project 3: Subreddit Classification



#### Executive Summary

Reddit is an American social news aggregation, web content rating, and discussion website

A *subreddit* is a specific online community, and the posts associated with it, on the social media website Reddit. *Subreddits* are dedicated to a particular topic that people write about, and they're denoted by /r/, followed by the subreddit's name, e.g., /r/gaming.

In this project, I looked into the two subreddit's namely: 'Silverbugs' and 'Gold'.
Gold and silver have been recognized as valuable metals, and have been coveted for centuries. 
Precious metals can be a good portfolio diversifier and hedge against inflation

In addition to owning physical metal, investors can also gain access through the derivatives market, 
metal ETFs and mutual funds, and mining company stocks

COVID-19 pandemic  have caused unprecedented volatility and fear in the financial markets. In mid March  2020, US Fed slashes rates to near zero, rolls out massive stimulus in an emergency response to the COVID-19 threat. Market uncertainty and lower economic growth is expected to drive precious metal demand.

<u>What is a silver or goldbug?</u>

Gold bug is a term frequently employed in the financial sector and among economists in reference to persons who are frantically bullish on the commodity gold as  an investment and who thinks its price will perpetually increase. Similarly,the term silverbug is explained in the same fashion except that focus is on silver.

### Define the problem.

I am a financial quantitative analyst with a Hedge fund company. Recently, our company is re-balancing our portfolio on precious metals and I was tasked to leverage on data to identify any trends, sentiments in precious metals sector. 

Specifically to, examining reddit's post and gaining insights to :

- which precious metal is best for investment purposes?
- Which form of precious metals investment (CFDs, ETFs, futures, gold related stock, physical holdings etc)
- Any other findings from post related stats like the number of comments of likes to a title post 



### Getting and cleaning data

The subreddit posts are uniquely identified based on the variable 'name'. After removing duplicates, I have 875 counts of subreddit Gold and 1041 counts of subreddit Silverbugs

From the original scrapped data, it is narrowed to the following: 'post_id','author','title','post_body','comments','num_comments','ups', 'subreddit’



### Models

The approach is to use a classification model and try to classify random posts into the correct subreddit category. 

The dependent variable for the classification model is 'subreddit' and the independent variables are: title and comments. The classifications models tired were Naive Bayes, Random Forest and Logistic Regression. 

Hyper-parameters tuning was done using GridSearchCV for Random Forest and Logistic Regression

The models score are as follows:

|                     | Train Score | Test Score |
| ------------------- | ----------- | ---------- |
| Naive Bayes         | 0.863       | 0.706      |
| Logistic Regression | 0.865       | 0.747      |
| Random Forest       | 0.967       | 0.745      |



The best model was the Logistics Regression with TfidfVectorizer, having a test accuracy of 74.7%



#### Findings & Recommendations

<u>Gold is preferred over Silver</u>

- Silverbugs also shows interest in gold. Conversely, Gold subreddit show very little in silver

- "Buy/Buying Gold" phrases at 135 counts vs "Buy Silver" at 29 counts. That’s almost more than 4.5 times more!

- Gold has 3 post titles related to physical holdings of gold('stacking'). Not just that, these posts are in the top 4 categories in terms of  number of comments and upvotes on these title On the other hand, Silver has just one related post title

- Sentiment analysis using TextBlob showed that subreddit Gold had a marginal higher positive sentiment than Silverbug.  This coincide with my phrases based analysis

  

<u>Trend on the physical holdings of precious metals(Gold and Silver)</u>

- On both Gold and Silver, physical holdings is most preferred

- In the Gold top40 phrases chart, there is only one mention on "Gold stock" vs 14 orange bars(terms that relate to physical holdings)

  

<u>Limitations:</u>

- Insufficient data to generate There isn’t enough data just from Reddit alone. Beside, it would be better to get data from dedicated precious metals sites like kitco.com  

- A further step into sentiment analysis could include manual or hybrid based algorithms instead of automatic ones



Sources: 

https://www.investopedia.com/articles/basics/09/precious-metals-gold-silver-platinum.asp

https://www.marketwatch.com/story/gold-as-an-investment-is-made-for-times-like-these-2020-05-05

https://www.dictionary.com/e/slang/subreddit/
https://www.investopedia.com/articles/basics/09/precious-metals-gold-silver-platinum.asp

https://www.investopedia.com/terms/g/goldbug.asp

