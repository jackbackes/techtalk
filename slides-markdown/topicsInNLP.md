# Natural Language Processing



# Natural Language Processing
## Topics
- Regular Expressions
- Word Tokenization
- Word Normalization and Stemming
- Sentence Segmentation
- Minimum Edit Distance
- Language Modeling
  - **_N-Grams_**


- Spelling Correction
  - Noisy Channel
  - Real-World
- Text Classification
  - Naive Bayes Algorithm
- Sentiment Analysis
- Modeling text features
- Maxent and NLP
- Information Extraction
  - Named Entity Recognition
  - Relation Extraction
    - Semi-supervised and supervised
- Parts of speech


- Syntactic Structure
- Context-free grammars
  - Probabalistic CFGs
  - Constituency Parsing
  - Lexicalization
- Information Retrieval
  - Term-Document Incidence Matrix
  - Inverted index
  - Querying
  - Ranked Retrieval and Term Ranking
  - Documents as Vectors


- Word Sense
  - Word Relation
  - Word Similarity
  - Thesauruses (Thesaurusi?)
- Question Answering
  - Query Formulation and Answer Types
  - Using Knowledge
  - Answering Complex Questions
- Summarization



# N-Grams
## What is it?
A Language Modeling Tool
> N = 1 : "Unigram (or, you know, a _word_)"
>  ie: "The"

> N = 2 : "Bigram"
>  ie: "The Cat"

> N = 3 : "Trigram", etc.
>  ie: "The Cat sat"

> N = 4 : "Four-gram", "Five-Gram", etc.


![HeShe](http://chrisharrison.net/projects/trigramviz/HESHEGraphWordsViz1.jpg) "He and She Trigrams"


![IYou](http://chrisharrison.net/projects/trigramviz/IYOUGraphWordsViz1.jpg) "I and You Trigrams"


| frequency | word1 | word2 | word3 |
| --------- | ----- | ----- | ----- |
| 1419 | much | the | same |
| 461 | much | more | likely |
| 432 | much | better | than |
| 266 | much | more | difficult |
| 235 | much | of | the |
| 226 | much | more | than |


# N-Grams
## When is it Useful?

When you use Markov chains!



# N-Grams and Markov Chains
"A [Markov chain](http://en.wikipedia.org/wiki/Markov_chain) is a probabilistic model well suited to [semi-coherent text synthesis](http://megahal.alioth.debian.org/How.html)."


Probability of word 2 given word 1 => P[w2|w1]


Markalvin and Hobbes
![alt-text](http://www.joshmillard.com/markov/calvin/images/calkov-4344106492004371456.jpg)


The Big Markovski
![alt-text](http://joshmillard.com/markov/lebowski/images/markovski-066901970.jpg)


Markov College Essays!
> Today’s world around me blind. In just one of energy. _While straining to live in Ukraine_ with anxiety and broad range of my surroundings, along the ones I felt physically threatened and the rush I burst into a ten-year old who they sought a poem that matters, I was I should be invincible. Who would have paid for granted, but maybe it was asked to further education is an annual overnight toSan Diego, water fun, cheers, a year, I still burn in the invisible enemy in the night when I cannot feel the traffic outside the times I want to a missionary would be neither relived nor reanimated. I assume the status quo, seems fair; I were a stylish figure, for me, and knees.


# The Markov Model
> The Probability of a word depends only the probability of the n-previous words.

![alt-text](http://sookocheff.com/img/nlp/ngram-modeling-with-markov-chains/learned-probabilities.png)



# N-Grams
## Where to Find them


## American English: Breakfast Foods
<iframe name="ngram_chart" src="https://books.google.com/ngrams/interactive_chart?content=pancake%2Cwaffle%2Cpeanut+butter%2Cmilkshake%2Corange+juice%2Cflaxseed&year_start=1900&year_end=2008&corpus=17&smoothing=3&share=&direct_url=t1%3B%2Cpancake%3B%2Cc0%3B.t1%3B%2Cwaffle%3B%2Cc0%3B.t1%3B%2Cpeanut%20butter%3B%2Cc0%3B.t1%3B%2Cmilkshake%3B%2Cc0%3B.t1%3B%2Corange%20juice%3B%2Cc0%3B.t1%3B%2Cflaxseed%3B%2Cc0" width=900 height=500 marginwidth=0 marginheight=0 hspace=0 vspace=0 frameborder=0 scrolling=no></iframe>


## British English, Breakfast Foods
<iframe name="ngram_chart" src="https://books.google.com/ngrams/interactive_chart?content=pancake%2Cwaffle%2Cpeanut+butter%2Cmilkshake%2Corange+juice%2Cflaxseed&year_start=1900&year_end=2008&corpus=18&smoothing=3&share=" width=900 height=500 marginwidth=0 marginheight=0 hspace=0 vspace=0 frameborder=0 scrolling=no></iframe>




# N-Grams
## How do you generate them from your text?


# Sequelize / PSQL example


# Normalization and Unigram Parse
```
// the default normalization rules
  let defaultRules = [
    // [regex,         'replace']
       [/\n/g,         '<n>'], // <== replaces carriage returns with a newline token
       [/[\.\!\?]/g, '<s>'] // <== replaces end of sentences with a phrase-stop token
  ]
// simple word and hyphenated word parser
// these can get a LOT more complicated
  let defaultPhraseParser = /[\w\-]+/g
// .parseText applies the normalization and
// parseMatcher Regular Expressions to the
// raw text
  Unigram.parseText( rawText, normalizationRules, parseMatcher)
```


# Normalization and Unigram Parse
## Result
```
tokens = [
  '<s>', 'i', 'am', 'sam', 'sam', 'i', 'am', '<s>''that', 'sam-i-am', 'that', 'sam-i-am', 'i', 'do', 'not', 'like', 'that', 'sam-i-am', '<s>', etc...
].map( token => {word: token} )
Word.bulkCreate(tokens) // is really fast ( 2-3 minutes )
```


# Bigram Parse and Training
## Tokenization
```
bigramMap = [ [ '<s>', tokens[0] ] ]
tokens.forEach( token, i, textArray => {
  if( i < textArray.length) bigramMap.push( token, token[textArray.length - 1] )
  else bigramMap.push( [token, '</s>'])
  })
// Bigram.bulkCreate takes a LOT longer because of  eager loading. 20-30 minutes.
```


# Trigram Parse and Training



# Implementing Mr. Markov
```
function wordTree( depth = 2, word = 'the', which = 'one' ) {
        if ( depth < 1 ) return;
        return $http.get( '/api/v1/ngram/bigram/' + word )
          .then( response => {
            let suggestions = response.data
              .map( bigram => bigram.tokens[ 1 ].word )
            let suggestion = suggestions[ Math.floor( Math.random() * suggestions.length ) ]
            let whichOne = 'wordTreeSuggestions' + which;
            scope[ whichOne ] += " " + suggestion;
            return wordTree( depth - 1, suggestion, which );
          } )
      };
```



# Resources
## Videos
## Libraries
## Datasets & Corpus


# Resources
## Videos
- Dan Jurafsky & Chris Manning of StanfordNLP:
  - [Natural Language Processing](https://www.youtube.com/playlist?list=PL6397E4B26D00A269): Free 100+ video walkthrough on the core of NLP. Watch it on the train.


# Resources
## Libraries
- [Stanford NLP on Github](https://github.com/stanfordnlp)

## Datasets & Corpus & Fun
- [Chris Harrison's Web Trigrams](http://www.chrisharrison.net/index.php/Visualizations/WebTrigrams)
- [Corpus of Contemporary English](http://www.ngrams.info/) - free and paid versions. 520,000,000 words.
- [Microsoft AI] (https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)
