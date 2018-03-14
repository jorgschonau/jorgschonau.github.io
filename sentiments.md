# Sentiment Analysis

For my final project at [Ubiqum Code Academy](https://goo.gl/aEmoTQ) I analysed a number of classic literature books, namely:

- [1984 by George Orwell](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four)
- [Crime and Punishment by Fyodor Dostoevsky](https://en.wikipedia.org/wiki/Crime_and_Punishment)
- [Charlie and the Chocolate Factory by Roald Dahl](https://en.wikipedia.org/wiki/Charlie_and_the_Chocolate_Factory)
- [Hitchhiker's Guide to the Galaxy by Douglas Adams](https://en.wikipedia.org/wiki/The_Hitchhiker%27s_Guide_to_the_Galaxy)

You'll find a number of visulations based on the data under the link further below.

## The Project

The sentiment analyses mainly involved firstly preparing the date importing text files, cleaning the data and re-arranging it into suitable formats. The second step was to a) choosing dictionary as basis for the sentiment analyses and b) re-aaranging the output data so that it Tableau could understand it. 

I'll explain here more about the details of the project, so text/ sentiment analysis, the dictionaries I used etc. But bascially I wanted to see how far one can use the standard dictionaries within R (NRC, Bing, AFINN & SentimentR) to analyse content (in this case books) and in how far the results differ.

In the end I decided to use the NRC dictionary for the detailed analysis. Mostly because it is the most extensive one and it produced overall most insightful information. The sentence based package SentimentR (the other dictionaries work on a word-by-word basis) produced also some good results, I noticed overall some surprising similarities with the ones generated with NRC. At a later stage I might compare both in more detail (depending on how much time I will on my hands hands). 

## Visualisations

Producing visualisations with Tableau made up a considerable part of the project, so without further ado, here they are:

[Visualisations](sentiments_multiple.html)

## Methodology

For more details about the actual text analysis

[Methodology](Methodology.md)
