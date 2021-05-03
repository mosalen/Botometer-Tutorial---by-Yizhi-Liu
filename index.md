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

In your Python shell or script, import Botometer and input the credentials we got earlier. We also import time because we might want to control the frequency later.
![image](https://user-images.githubusercontent.com/42571555/116944482-70712800-ac2a-11eb-85a6-67f6abd02d07.png)

Then, instantiate Botometer:
![image](https://user-images.githubusercontent.com/42571555/116944679-ccd44780-ac2a-11eb-86cb-991e430eca75.png)



```markdown
# RapidAPI

First, go to [Link](https://rapidapi.com/OSoMe/api/botometer-pro) and create a RapidAPI account first.
Then, click the "Subscribe to Test" button, you will see the page below.
![image](https://user-images.githubusercontent.com/42571555/116942513-94326f00-ac26-11eb-965f-bf5de9557cbf.png)

For testing and exploration purpose, we can select the Basic plan. Don't worry, it's completely free!
If you need more quota in the future, you can switch to a higher plan at any time.

Now, let's go back to [Link](https://rapidapi.com/OSoMe/api/botometer-pro) and you will see the API information.
![image](https://user-images.githubusercontent.com/42571555/116942186-fd65b280-ac25-11eb-9b4e-0e1c1dc235b8.png)

Please keep X-RapidAPI-Key properly. We will need it soon.

## Twitter 
To apply for a Twitter developer account, go to [Link](https://developer.twitter.com/)
There are plenty of tutorials out there, so we will not go through the details.

After you get the account and create an app, you will have the following page:
![image](https://user-images.githubusercontent.com/42571555/116942911-5bdf6080-ac27-11eb-88ad-8cb7be30c8a4.png)

We will need all these information: consumer_key, consumer_secret, access_token, access_token_secret

Again, please keep them properly. You need to regenerate them again if they are lost.

### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```





For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/mosalen/LING539-YL.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
