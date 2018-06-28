# Methodology

## Crime and Sentiment
Below I'll elaborate more about the sentiment analysis project, the steps I've done, problems I encountered, things I learnt etc.

## Github
You'll find the R code here: [https://github.com/jorgschonau/finalproject/blob/master/TextAnalysis](https://github.com/jorgschonau/finalproject/blob/master/TextAnalysis)

## What are actually sentiments?
Sentiment detection (or sentiment analysis) is a sub area of text mining and refers to the automatic evaluation of texts with the objective of detecting whether opinions/ feelings expressed within a text are positive or negative. Even though sentiment is often framed as binary distinction (so positive vs. negative), it can be more fine grained. For example it is also possible to try and identify the specific emotions within a text (for example fear, joy, trust or sadness).

Some examples of applications for sentiment analysis include:

- Analyzing social media discussions (such as Twitter) around certain topics 
- Determining whether product reviews (for example on Amazon) are positive or negative
- Evaluating opions expressed in newspaper articles regarding certain topics or persons

In nutshell, what happens during a sentiment analysis is that a dictionary containing a range of words (for instance, the NRC dictionary has over 13,900 entries) classified as positive or negative is being matched against a text of your choice. 
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

That's more like it: 8x negative vs 1x positive, so a total score of -7, which does match with the atmosphere of doom & gloom Dostojewski builds up so masterfully right from the first page onwards.

As you might already suspect, sentiment analysis is far from being perfect and the word-by-word approach is indeed somewhat crude. As with any automatic analysis of language, there are bound to be errors in the results. It also can't tell you why a writer is feeling a certain way and it also fails to pick up irony, humour etc. Another big problem of the dictionary based, word-by-word evaluation is that it fails to consider negations. So a sentence like "I am having a great day" would receive the same score as "I am __not__ having a great day".
The sentence-based SentimentR package can capture simple negations like the one above, but would still fail to detect the meaning of a sentence such as: "I am __far from__ having a great day".

For some text sections the sentiment scoring is completely off, I'll list some examples on the next page.

Despite these shortcomings, sentiments analysis can be useful to quickly summarize some qualities of text, especially if you are dealing with large volumes of text (think thousands and tens of thousand web pages or more), it would be impossible for a human reader to analyze all of it within a reasonable time.


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

## Importing & Cleaning Text Files

Some books I found directly in a .txt format on [Project Gutenberg](http://www.gutenberg.org/). For others I had to find the PDF files somehwere online and then simply copied & pasted the text into an editor and saved the file as .txt.

```
# loading text file 
charlieandthechocolatefactory_raw <- read_lines("charlieandthechocolatefactory.txt") # read txt file
```

I then converted the raw text into the tidy text format. Basically it's a table with one token (aka meaningful word) per row. This requires follwing steps: 

```
# cleaning & converting into tidytext
charlieandthechocolatefactory_clean <- cleanup(charlieandthechocolatefactory_raw)
charlieandthechocolatefactory_df <- data_frame(line = 1:2518,text = charlieandthechocolatefactory_clean)
charlieandthechocolatefactory <- charlieandthechocolatefactory_df %>%
            mutate(linenumber = row_number(), chapter = cumsum(str_detect(text, regex("^chapter [\\divxlc]", 
            ignore_case = TRUE))))
```

"Cleanup" is a function I wrote that simply combines a number of steps for cleaning up the text and getting it into a standard format. These steps are:

 - tolower (part of base R): all characters into lower case
 - replace_word_elongation (part of textclean package): Replacing elongations like: "I said heyyy!', "Wwwhhatttt!", or "Nooooooooo!"
 - replace_contraction (part of textclean package): Replacing contractions such as "can't", didn't", "won't" by the full form ("can not", "did not", "will not")
 - strwrap part of base R): wrapping character strings to format paragraphs after a given length of characters

Below the cleanup function:

```
cleanup <- function(x) {
  library(textclean)
  result <- tolower(x)
  result <- replace_word_elongation(result)
  result <- replace_contraction(result)
  result <- strwrap(result, width = 72)
  return(result)
}
```

The cleaned up text is then stripped off all stopwords (stop_words is part of the Tidytext package and is a collection of 3 dictionaries) and transformed into the one-token-per-row Tidytext format:

```
tidy_orwell1984 <- orwell1984 %>% unnest_tokens(word, text) %>% anti_join(stop_words)
```

In hindsight, using the entire stop_words list seems to be somewhat excessive. As a consequence, words that do carry sentiments, such as "interesting" or "young", have been excluded. It actually turned out later that removing stopwords wasn't necessary at all as stopwords don't interfere in the sentiment scoring. So if I was to start a similar project again, I would either refrain from removing stopwords alltogether or otherweise I would create a sub set of the stopwords dictionary and only use 1 or 2 of the sub dictionaries.

## Tidytext Format

So after executing the steps above, the text will be transformed into a token based dataframe and all stopwords are removed. To illustrate, the first two sentences of Charlie and the Chocolate factory:

>These two very old people are the father and mother of Mr Bucket. Their names are Grandpa Joe and Grandma Josephine.

are being transformed into this format:

| Line Number  | Chapter     | Word      |
|-------------:|:-----------:|:---------:|
|  5           | 1           | people    |
|  5           | 1           | father    |
|  6           | 1           | mother    |
|  6           | 1           | bucket    |
|  6           | 1           | names     |
|  6           | 1           | grandpa   |
|  6           | 1           | joe       |  
|  6           | 1           | grandma   |
|  7           | 1           | josephine |


[NEXT-Issues & ambiguities](sentiments_issues.md)

