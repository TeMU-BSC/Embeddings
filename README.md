# Embeddings

This repository contains the word embeddings generated from Spanish corpora.

## Introduction

In recent years word embedding/distributional semantic models evolved to become a fundamental component in many natural language processing (NLP) architectures due to their ability of capturing and quantifying semantic associations at scale. Word embedding models can be used to satisfy recurrent tasks in NLP such as lexical and semantic generalisation in machine learning tasks, finding similar or related words and computing semantic relatedness of terms.

However, building and consuming specific word embedding models require
the setting of a large set of configurations, such as corpus-dependant parameters, distance measures as well as compositional models.

## Corpora used

* Scielo Full-Text in Spanish: We retrieved all the full-text available in Scielo.org (until December/2018) and processed them into sentences. Scielo.org node contains all Spanish articles, thus includes Latin and European Spanish.
  * Sentences: ?
  * Tokens: ?
* Wikipedia Health: We retrieved all articles from the following Wikipedia categories: Pharmacology, Pharmacy, Medicine and Biology. Data were retrieved during December/2018.
  * Sentences: ?
  * Tokens: ?
* Scielo + Wikipedia Health: We concatenated the previous two corpora.


## Preprocessing Step

1) Removing not Spanish sentences by [Lingua] (https://github.com/pemistahl/lingua) library.
But because it does not work with high accuracy for Spanish langugae, we used a reversed way by removing English, German, French, Dutch and Danish sentences from our retrieved Spanish corpora.

2) Sentence spliting and tokenizing by [Freeling](http://nlp.lsi.upc.edu/freeling/) toolkit.

**FREELING** (Padro and Stanilovsky, 2012) is a C++ library providing language analysis functionalities 
(Morphological Analysis, Named Entity Detection, PoS-Tagging, Parsing, Word Sense Disambiguation, 
Semantic Role Labelling, so forth) for a variety of languages.

3) Final pre-processing by [Indra-Indexer](https://github.com/Lambda-3/Indraindexer) toolkit. 

By Indra:
	1) Seperating punctuations from tokens.
	2) Replaceing numbers for the place holder -> <NUMBER>.
	3) Setting a minimum acceptable token size to 1.
	4) Setting a maximum acceptable token size to 100.
	5) Applying lowercase the tokens (we generated both Cased and Uncased version).

## Embeddings generated

### fastText

We used the fastText (https://fasttext.cc/) to train word embeddings.
We kept all standard options for training.

• Minimum number of word occurrences: 5
• Phrase representation: No (i.e. length of word n-gram = 1)
• Minimum length of character n-gram: 3
• Maximum length of character n-gram: 6
• Size of word vectors: 300
• Epochs: 20
• Size of the context window: 5
• Word-Representation Modes: CBOW and SKIPGRAM

The following corpora were used: Scielo, Wikipedia and Scielo + Wikipedia

We generated word embedding for both version (Cased and Uncased corpora):

- Scielo_cased
- Scielo_uncased

- Scielo+Wiki_cased
- Scielo+Wiki_uncased

- Wiki_cased
- Wiki_uncased

### Evaluation



## Directory Structure




## Digital Object Identifier (DOI) and access to dataset files

=======


## Contact

Siamak Barzegar (siamak.barzegar@bsc.es)


## License

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

Copyright (c) 2018 Secretaría de Estado para el Avance Digital (SEAD)
