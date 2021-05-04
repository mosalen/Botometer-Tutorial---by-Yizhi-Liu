# Introduction: What are social bots and what is Botometer?

Have you noticed any bots on social media?

It is estimated that 9-15% of active Twitter accounts may be social bots. They are EVERYWHERE!

![image](https://user-images.githubusercontent.com/42571555/116933847-f8e6cd00-ac18-11eb-8acf-a14ae75816d5.png)

Many kinds of social bots exist in social media. Malicious bots can be used to manipulate social media users by amplifying misinformation, creating the appearance that some people, ideas, or products are more popular than they are, committing financial fraud, suppressing or disrupting speech, spreading malware or spam, trolling/attacking victims, and other types of abuse.

![image](https://user-images.githubusercontent.com/42571555/116934258-917d4d00-ac19-11eb-8f59-00d4362c14d9.png)

Botometer is one of the most famous and powerful social bot detection tools that are public available. It is developed as a machine learning algorithm to detect social bots. It is trained to calculate a "bot score" where low scores indicate likely human accounts and high scores indicate likely bot accounts.

In this tutorial, we will go through the whole process of using Botometer to detect social bots on Twitter. This includes how to collect data from Twitter, store them in your local machine, and leverage Botometer to detect the soical bots in the data you collected. In addition, to evaluate the performance, we will also compare Botometer with some deep learning methods (e.g., LSTM) using pulicly labeled data.


## 1. Setup

In this tutorial, we are using Python 3.6, and other configures are listed below.
Note that requests and tweepy are required.

```markdown
### Install requests and tweepy if not exist
pip install requests tweepy

### Other information
setup(name='botometer',
      version='1.6',
      description='Check Twitter accounts for bot behavior',
      url='https://github.com/IUNetSci/botometer-python',
      download_url='https://github.com/IUNetSci/botometer-python/archive/1.0.zip',
      author='Clayton A Davis, Kai-Cheng Yang',
      author_email='claydavi@indiana.edu,yangkc@iu.edu',
      license='MIT',
      packages=['botometer'],
      install_requires=[
          'requests',
          'tweepy >= 3.5.0',
          ],
      )
```


## 2. API Preparations

Before using Botometer, we need to make some prepartions to get the API authorization.
Botometer provides its public service through RapidAPI.
Also, Twitter APP key is needed because we need to get access to Twitter user data in a legal way.


### RapidAPI

First, go to [Link](https://rapidapi.com/OSoMe/api/botometer-pro) and create a RapidAPI account first.
Then, click the "Subscribe to Test" button, you will see the page below.
![image](https://user-images.githubusercontent.com/42571555/116942513-94326f00-ac26-11eb-965f-bf5de9557cbf.png)

For testing and exploration purpose, we can select the Basic plan. Don't worry, it's completely free!
If you need more quota in the future, you can switch to a higher plan at any time.

Now, let's go back to [Link](https://rapidapi.com/OSoMe/api/botometer-pro) and you will see the API information.
![image](https://user-images.githubusercontent.com/42571555/116942186-fd65b280-ac25-11eb-9b4e-0e1c1dc235b8.png)

Please keep X-RapidAPI-Key properly. We will need it soon.


### Twitter 
To apply for a Twitter developer account, go to [Link](https://developer.twitter.com/)
There are plenty of tutorials out there, so we will not go through the details.

After you get the account and create an app, you will have the following page:
![image](https://user-images.githubusercontent.com/42571555/116942911-5bdf6080-ac27-11eb-88ad-8cb7be30c8a4.png)

We will need all these information: consumer_key, consumer_secret, access_token, access_token_secret

Again, please keep them properly. You need to regenerate them again if they are lost.



## 3. Quick Start

Now, let's start using Botometer with a simple example!

First, install Botometer in your command shell.
```markdown
pip install botometer
```

In your Python shell or script, import Botometer and input the credentials we got earlier. 
We also import time because we might want to control the frequency later.

![image](https://user-images.githubusercontent.com/42571555/116944482-70712800-ac2a-11eb-85a6-67f6abd02d07.png)

Then, instantiate Botometer:

![image](https://user-images.githubusercontent.com/42571555/116944679-ccd44780-ac2a-11eb-86cb-991e430eca75.png)


### Try to Check Accounts
Let's try some accounts! Botometer allows searching with account names or ids. 
```markdown
result = bom.check_account('@BarackObama')
print(result)
```
So here are the results of @BarackObama:
```markdown
{'cap': {'english': 0.7971612475312038, 'universal': 0.8052444082217497}, 

'display_scores': 
{'english': {'astroturf': 2.1, 'fake_follower': 1.6, 'financial': 0.0, 'other': 2.2, 'overall': 2.5, 'self_declared': 0.4, 'spammer': 0.2}, 
'universal': {'astroturf': 1.8, 'fake_follower': 1.6, 'financial': 0.0, 'other': 1.4, 'overall': 3.0, 'self_declared': 0.2, 'spammer': 0.0}}, 

'raw_scores': 
{'english': {'astroturf': 0.42, 'fake_follower': 0.33, 'financial': 0.0, 'other': 0.44, 'overall': 0.5, 'self_declared': 0.09, 'spammer': 0.03}, 
'universal': {'astroturf': 0.37, 'fake_follower': 0.32, 'financial': 0.0, 'other': 0.27, 'overall': 0.6, 'self_declared': 0.05, 'spammer': 0.0}}, 

'user': {'majority_lang': 'en', 'user_data': {'id_str': '813286', 'screen_name': 'BarackObama'}}}
```

### Meanings of Returned Results
Well, there is a lot of information. What do they mean? The meanings of the four elements are:
```markdown
user: Twitter user object (from the user) plus the language inferred from majority of tweets
raw scores: bot score in the [0,1] range, both using English (all features) and Universal (language-independent) features; in each case we have the overall score and the sub-scores for each bot class (see below for subclass names and definitions)
display scores: same as raw scores, but in the [0,5] range
cap: conditional probability that accounts with a score equal to or greater than this are automated; based on inferred language
```

And the meanings of the bot type scores are:
```markdown
fake_follower: bots purchased to increase follower counts
self_declared: bots from botwiki.org
astroturf: manually labeled political bots and accounts involved in follow trains that systematically delete content
spammer: accounts labeled as spambots from several datasets
financial: bots that post using cashtags
other: miscellaneous other bots obtained from manual annotation, user feedback, etc.
```
In short, if we look at "raw_scores" and "English," the account @BarackObama is not a bot, but its 'astroturf' score is relatively high.
If we look at the definition of "astroturf," you may think the results indeed make sense. 

Let's try another account using the id:
```markdown
result2 = bom.check_account(27260086)
print(result2)

{'cap': {'english': 0.7668769621923945, 'universal': 0.8035432096295109}, 

'display_scores': 
{'english': {'astroturf': 1.8, 'fake_follower': 1.8, 'financial': 0.0, 'other': 1.9, 'overall': 1.4, 'self_declared': 0.1, 'spammer': 0.0}, 
'universal': {'astroturf': 1.6, 'fake_follower': 1.6, 'financial': 0.0, 'other': 1.6, 'overall': 2.0, 'self_declared': 0.0, 'spammer': 0.0}}, 

'raw_scores': 
{'english': {'astroturf': 0.37, 'fake_follower': 0.36, 'financial': 0.0, 'other': 0.38, 'overall': 0.28, 'self_declared': 0.02, 'spammer': 0.0}, 
'universal': {'astroturf': 0.32, 'fake_follower': 0.32, 'financial': 0.0, 'other': 0.32, 'overall': 0.41, 'self_declared': 0.01, 'spammer': 0.0}}, 

'user': {'majority_lang': 'en', 'user_data': {'id_str': '27260086', 'screen_name': 'justinbieber'}}}
```
Great! Next, let's test social bots in scale and see how Botometer perform.



## 4. Accuracy Evaluation
### Check Accounts in Bulk
Botometer does allow us to check accounts in bulk. Howvever, that requires a Pro subscription and above.
Therefore, we can write a script to bypass it and check accounts one by one automatically.

```markdown
file_name = 'path to the account text file.'
user_list = open(file_name, encoding='utf-8').read().split('\n')


num = 0
for account in user_list:
    ram = random.randint(0, 3)
    time.sleep(ram)

    try:
        result = bom.check_account(account)
        print("result get!")
        num += 1
        score = result.get('raw_scores')
        score = score.get('english')
        score = score.get('overall')
        name = result.get('user')
        name = name.get('user_data')
        name = name.get('screen_name')

        bot_result = open('path to the result text file.', "a")
        bot_result.write(str(name) + ' ')
        bot_result.write(str(score))
        bot_result.write("\n")
        bot_result.close()
        print(num)
    except Exception as e:
        print(str(e))
        print(str(account) + " may not exist any more!")
        bot_result = open('path to the result text file.', "a")
        bot_result.write("N/A")
        bot_result.write("\n")
        bot_result.close()
        pass
```

Where can we get the account lists? We surely can obtain it by collecting from Twitter.
However, to evaluate the performance of Botometer, we need some labeled data.

### Ground-truth Data
Labeled social bot account data is not easy to obtain, because it needs a lot of human effort. 
Also, Twitter often bans those social bots, making some labeled data invalid.

Fortunately, here is a relatively new dataset on Kaggle: [Link](https://botometer.osome.iu.edu/bot-repository/datasets.html)

Since Botometer API only allows 500 quota per day, we randomly select 500 accounts from this data.

### Evaluation
Among the 500 accounts, there are still 100 accounts that are either not accessible or already banned by Twitter.
400 accounts are valid and can be analyzed by Botometer.
![image](https://user-images.githubusercontent.com/42571555/116966642-7fc19700-ac65-11eb-84ae-6c84b1c78310.png)

After evaluation, there are 337 results are correct, and 63 are wrong.
![image](https://user-images.githubusercontent.com/42571555/116967606-8e10b280-ac67-11eb-8bef-652f8b850012.png)

Let's look at the confusion matrix to investigate the wrong predictions.
![image](https://user-images.githubusercontent.com/42571555/116968454-586cc900-ac69-11eb-8a2e-02a99e287cb2.png)

From the confusion matrix, we see that Botometer can get a decent performance.
However, it wrongly classified quite a few human accounts as bots, leading to a lower precision.


## 5. Conclusion
In this tutorial, we go through how to leverage Botometer, one of the most advanced social bot detection tool, to detect social bots in Twitter.
We can see from the results that Botometer has a good performance on real-world data. However, it tends to get relatively high false positives.
In the future, we can investigate what features Botometer and other similar tools use, which may help us to improve their performance.

