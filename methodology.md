# Methodology
Below I'll elaborate more about the sentiment analysis project, the steps I've done, problems I encountered and any learnings. 

## What are actually sentiments?
Sentiment detection (or sentiment analysis) is a sub area of text mining and referes to the automatic evaluation of texts with the objective of detecting whether opinions/ feelings expressed within a text are positive or negative. Even though sentiment is often framed as binary distinction (so positive vs. negative), it can be more fine grained. For example it is also possible to try and indentify the specific emotions within a text (such as fear, joy, trust or sadness).

Some examples of applications for sentiment analysis include:

- Analyzing social media discussions (such as Twitter) around certain topics 
- Determining whether product reviews (for example on Amazon) are positive or negative
- Evaluating opions expressed in newspaper articles regarding certain topics or persons

In nutshell, what happens during a sentiment analysis is that you match a dictionary cotaining range of words classified as positive or negtive against a text of your choice (or an entire text collection, also known as corpus). 
Let's take the first sentence of the book 1984 as an example to illustrate:

*It was a bright cold day in April, and the clocks were striking thirteen.*

The NRC dictionary (more about this later) would evaluate this sentence like following:

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


Sentiment analysis is not perfect, and as with any automatic analysis of language, you will have errors in your results. It also cannot tell you why a writer is feeling a certain way. However, it can be useful to quickly summarize some qualities of text, especially if you have so much text that a human reader cannot analyze all of it.

The sentiment analyses mainly involved firstly preparing the date importing text files, cleaning the data and re-arranging it into suitable formats. The second step was to a) choosing dictionary as basis for the sentiment analyses and b) re-arranging the output data so that it Tableau could understand it. 

## Crime & Sentiment
I'll explain here more about the details of the project, so text/ sentiment analysis, the dictionaries I used etc. But bascially I wanted to see how far one can use the standard dictionaries within R (NRC, Bing, AFINN & SentimentR) to analyse content (in this case books) and in how far the results differ.

I'll also provide the link to the github repo (first I need to clean up the code though)
