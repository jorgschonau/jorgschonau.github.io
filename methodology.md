# Methodology

## Crime and Sentiment
Below I'll elaborate more about the sentiment analysis project, the steps I've done, problems I encountered and any learnings. 

## What are actually sentiments?
Sentiment detection (or sentiment analysis) is a sub area of text mining and refers to the automatic evaluation of texts with the objective of detecting whether opinions/ feelings expressed within a text are positive or negative. Even though sentiment is often framed as binary distinction (so positive vs. negative), it can be more fine grained. For example it is also possible to try and identify the specific emotions within a text (for example fear, joy, trust or sadness).

Some examples of applications for sentiment analysis include:

- Analyzing social media discussions (such as Twitter) around certain topics 
- Determining whether product reviews (for example on Amazon) are positive or negative
- Evaluating opions expressed in newspaper articles regarding certain topics or persons

In nutshell, what happens during a sentiment analysis is that a dictionary containing a range of words (for instance, the NRC dictionary has 13901 entries) classified as positive or negative is being matched against a text of your choice. 
To illustrate, let's take the first sentence of the book 1984 as an example:

>It was a bright cold day in April, and the clocks were striking
>thirteen. Winston Smith, his chin nuzzled into his
>breast in an effort to escape the vile wind, slipped quickly
>through the glass doors of Victory Mansions, though not
>quickly enough to prevent a swirl of gritty dust from entering
>along with him.

The NRC dictionary (more about this later) would evaluate this sentence like following:

| Word          | Sentiment     |
| ------------- |:-------------:|
| cold          | -1            |
| striking      |  1            |
| effort        |  1            |
| dust          | -1            |


So the total sum for this particular paragraph in terms of sentiments would be 0 - completely neutral. Let's have a look at another piece of text, this time the first paragraph in Crime & Punishment:

>On an exceptionally hot evening early in July a young
>man came out of the garret in which he lodged in S. Place
>and walked slowly, as though in hesitation, towards K.
>bridge.
>He had successfully avoided meeting his landlady on
>the staircase. His garret was under the roof of a high, fivestoried
>house and was more like a cupboard than a room.
>The landlady who provided him with garret, dinners, and
>attendance, lived on the floor below, and every time he
>went out he was obliged to pass her kitchen, the door of
>which invariably stood open. And each time he passed, the
>young man had a sick, frightened feeling, which made him
>scowl and feel ashamed. He was hopelessly in debt to his
>landlady, and was afraid of meeting her.

The NRC dictionary evaluates the text above like following:

| Word          | Sentiment     |
| ------------- |:-------------:|
| hesitation    | -1            |
| invariably    |  1            |
| sick          | -1            |
| frightened    | -1            |
| ashamed       | -1            |
| hopeless      | -1            |
| debt          | -1            |
| afraid        | -1            |

That's more like it: 8x negative vs 1x positive, so a total score of -7, which does match with the atmosphere of doom & gloom Dostojewski builds up so masterfully right from the start.

As you might already suspect, sentiment analysis is far from being perfect and the word-by-word approach is indeed somewhat crude. As with any automatic analysis of language, there are bound to be errors in the results. It also can't tell you why a writer is feeling a certain way and it also fails to pick up irony, humour etc. Another big problem of the dictionary based, word-by-word evaluation is that it fails to consider negations. So a sentence like "I am having a great day" would receive the same score as "I am __not__ having a great day". The sentence-based SentimentR package can capture simple negations like that, but would still fail to detect the meaning of a sentence like "I am __far from__ having a great day". 

Despite these shortcomings, sentiments analysis can be useful to quickly summarize some qualities of text, especially if you have large volumes of text (think thousands and tens of thousand web pages or more) that it would be impossible for a human reader to analyze all of it within a reasonable time.


## Preparations

### Defining Packages
Below the packages I used for this project:

```
library(tidyverse)
library(readr)
library(reshape2)
library(stringr)
library(syuzhet)
library(viridis)
library(SentimentAnalysis)
library(RSentiment)
library(sentimentr)
library(tidytext)
library(tm)
library(SnowballC)
library(wordcloud)
library(RColorBrewer)
library(textclean)
library(widyr)
library(igraph)
library(ggraph)
library(viridis)
library(zoo)
library(tokenizers)
```



I'll explain here more about the details of the project, so text/ sentiment analysis, the dictionaries I used etc. But bascially I wanted to see how far one can use the standard dictionaries within R (NRC, Bing, AFINN & SentimentR) to analyse content (in this case books) and in how far the results differ.

I'll also provide the link to the github repo (first I need to clean up the code though)


