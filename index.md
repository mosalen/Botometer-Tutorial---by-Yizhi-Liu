## Introduction: What are social bots and what is Botometer?

Have you noticed any bots on social media?

It is estimated that 9-15% of active Twitter accounts may be social bots. They are EVERYWHERE!

![image](https://user-images.githubusercontent.com/42571555/116933847-f8e6cd00-ac18-11eb-8acf-a14ae75816d5.png)

Many kinds of social bots exist in social media. Malicious bots can be used to manipulate social media users by amplifying misinformation, creating the appearance that some people, ideas, or products are more popular than they are, committing financial fraud, suppressing or disrupting speech, spreading malware or spam, trolling/attacking victims, and other types of abuse.

![image](https://user-images.githubusercontent.com/42571555/116934258-917d4d00-ac19-11eb-8f59-00d4362c14d9.png)

Botometer is one of the most famous and powerful social bot detection tools that are public available. It is developed as a machine learning algorithm to detect social bots. It is trained to calculate a "bot score" where low scores indicate likely human accounts and high scores indicate likely bot accounts.

In this tutorial, we will go through the whole process of using Botometer to detect social bots on Twitter. This includes how to collect data from Twitter, store them in your local machine, and leverage Botometer to detect the soical bots in the data you collected. In addition, to evaluate the performance, we will also compare Botometer with some deep learning methods (e.g., LSTM) using pulicly labeled data.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
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
