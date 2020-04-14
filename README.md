# Embeddings V.2

This repository contains the word embeddings generated from Spanish corpora.

The first version is accessible from [this repository](https://github.com/PlanTL-SANIDAD/Embeddings). 
The used Corpora in the second version are preprocessed, and therefore the pre-trained models have more quality. 

## Corpora used

* **Scielo Full-Text in Spanish**: We retrieved all the full-text available in Scielo.org (until December/2018) and processed them into sentences. Scielo.org node contains all Spanish articles, thus includes Latin and European Spanish.
  * Sentences: 3,267,556
  * Tokens: 100,116,298
* **Wikipedia Health**: We retrieved all articles from the following Wikipedia categories: Pharmacology, Pharmacy, Medicine and Biology. Data were retrieved during December/2018.
  * Sentences: 4,030,833
  * Tokens: 82,006,270
* **Scielo + Wikipedia Health**: We concatenated the previous two corpora.


## Preprocessing Step

1) Removing not Spanish sentences by [Lingua](https://github.com/pemistahl/lingua) library.
	- Because it does not work with high accuracy for the Spanish language, we used a reversed way by removing English, German, French, Dutch and Danish sentences from our retrieved Spanish corpora.

2) Sentence splitting and tokenizing by [Freeling](http://nlp.lsi.upc.edu/freeling/) toolkit.
   - **Freeling** (Padro and Stanilovsky, 2012) is a C++ library providing language analysis functionalities  (Morphological Analysis, Named Entity Detection, PoS-Tagging, Parsing, Word Sense Disambiguation, Semantic Role Labelling, so forth) for a variety of languages.

3) Final pre-processing by [Indra-Indexer](https://github.com/Lambda-3/Indraindexer) toolkit. 
   - Removing punctuations.
   - Replacing numbers for the place holder `<NUMBER>`.
   - Setting a minimum acceptable token size to 1.
   - Setting a maximum acceptable token size to 100.
   - Applying lowercase the tokens (we generated both Cased and Uncased version).


**After preprocessing**:

|Corpora|# Sentences|# Unique Tokens (Cased)|# Unique Tokens (Uncased)|
|--------|-----|------|------|
|Scielo|9,116,784|385,399|335,790|
|Wikipedia Health|4,623,438|255,057|222,954|
|Scielo + Wikipedia Health|13,740,222|503,647|435,652|


## Embeddings generated

### fastText

We used the fastText (https://fasttext.cc/) to train word embeddings.
We kept all standard options for training.
<pre>
- Minimum number of word occurrences: 5
- Phrase representation: No (i.e. length of word n-gram = 1)
- Minimum length of character n-gram: 3
- Maximum length of character n-gram: 6
- Size of word vectors: 300
- Epochs: 20
- Size of the context window: 5
- Word-Representation Modes: CBOW and SKIPGRAM
</pre>

We generated word embedding for both version (Cased and Uncased corpora):
<pre>
- Scielo_cased
- Scielo_uncased
- Scielo+Wiki_cased
- Scielo+Wiki_uncased
- Wiki_cased
- Wiki_uncased
</pre>
## Evaluation

The evaluation was carried out by both extrinsic (with a Named Entity Recognition framework) and intrinsic, with the three already available datasets for such task UMNSRS-sim, UMNSRS-rel, and MayoSRS.
With NER, we defined that the best model was with 300 dimensions, and projected the words using Principal Component Analysis.

Further details about evaluation and the steps performed can be found in [Biomedical Word Embeddings for Spanish Development and Evaluation.pdf](https://www.aclweb.org/anthology/W19-1916.pdf)
### SKIPGRAM:

|Model|Validation|Test|
|--------|-----|------|
|Scielo_cased|89.66|88.77|
|**Scielo_uncased**|89.07 |**89.17**|
|Scielo+Wiki_cased|88.76|88.64|
|Scielo+Wiki_uncased|89.71|89.74|
|Wiki_cased|88.82|87.16|
|Wiki_uncased|88.77|87.21|

### CBOW:
|Model|Validation|Test|
|--------|-----|------|
|Scielo_cased|88.11|87.75|
|Scielo_uncased|89.99|87.24|
|Scielo+Wiki_cased|87.95|87.78|
|Scielo+Wiki_uncased|86.56|88.10|
|Wiki_cased|86.55|85.46|
|Wiki_uncased|85.12|85.74|


|      |       |          | Cased      |       | Uncased    |       |
|---------|----------|-------------|------------|-------|------------|-------|
| Version | Method   | Corpora     | Validation | Test  | Validation | Test  |
| 1.0    | skipgram | wiki        | 88.55      | 87.78 | -          | -     |
|         |          | scielo      | 89.47      | 87.31 | -          | -     |
|         |          | scielo+wiki | 89.42      | **88.17** | -          | -     |
| 2.0    | cbow     | wiki        | 86.55      | 85.46 | 85.12      | 85.74 |
|         |          | scielo      | 88.11      | 87.75 | 89.99      | 87.24 |
|         |          | scielo+wiki | 87.95      | 87.78 | 86.56      | 88.10 |
|         | skipgram | wiki        | 88.82      | 87.16 | 88.77      | 87.21 |
|         |          | scielo      | 89.66      | 88.77 | 89.07      | 89.17 |
|         |          | scielo+wiki | 88.76      | 88.64 | 89.71      | **89.74** |


<table class="tg">
  <tr>
    <th class="tg-0pky" colspan="3"></th>
    <th class="tg-7btt" colspan="2">Cased</th>
    <th class="tg-7btt" colspan="2">Uncased</th>
  </tr>
  <tr>
    <th class="tg-fymr">Version</th>
    <th class="tg-fymr">Method</th>
    <th class="tg-fymr">Corpora</th>
    <th class="tg-fymr">Validation</th>
    <th class="tg-fymr">Test</th>
    <th class="tg-fymr">Validation</th>
    <th class="tg-fymr">Test</th>
  </tr>
  <tr>
    <td class="tg-0pky" rowspan="3">v1.0</td>
    <td class="tg-0pky" rowspan="3">skipgram</td>
    <td class="tg-0pky">wiki</td>
    <td class="tg-0pky">88.55</td>
    <td class="tg-0pky">87.78</td>
    <td class="tg-0pky">-</td>
    <td class="tg-0pky">-</td>
  </tr>
  <tr>
    <td class="tg-0pky">scielo</td>
    <td class="tg-0pky">89.47</td>
    <td class="tg-0pky">87.31</td>
    <td class="tg-0pky">-</td>
    <td class="tg-0pky">-</td>
  </tr>
  <tr>
    <td class="tg-0pky">scielo+wiki</td>
    <td class="tg-0pky">89.42</td>
    <td class="tg-0pky">88.17</td>
    <td class="tg-0pky">-</td>
    <td class="tg-0pky">-</td>
  </tr>
  <tr>
    <td class="tg-0pky" rowspan="6">v2.0</td>
    <td class="tg-0pky" rowspan="3">cbow</td>
    <td class="tg-0pky">wiki</td>
    <td class="tg-0pky">86.55</td>
    <td class="tg-0pky">85.46</td>
    <td class="tg-0pky">85.12</td>
    <td class="tg-0pky">85.74</td>
  </tr>
  <tr>
    <td class="tg-0pky">scielo</td>
    <td class="tg-0pky">88.11</td>
    <td class="tg-0pky">87.75</td>
    <td class="tg-0pky">89.99</td>
    <td class="tg-0pky">87.24</td>
  </tr>
  <tr>
    <td class="tg-0pky">scielo+wiki</td>
    <td class="tg-0pky">87.95</td>
    <td class="tg-0pky">87.78</td>
    <td class="tg-0pky">86.56</td>
    <td class="tg-0pky">88.10</td>
  </tr>
  <tr>
    <td class="tg-0pky" rowspan="3">skipgram</td>
    <td class="tg-0pky">wiki</td>
    <td class="tg-0pky">88.82</td>
    <td class="tg-0pky">87.16</td>
    <td class="tg-0pky">88.77</td>
    <td class="tg-0pky">87.21</td>
  </tr>
  <tr>
    <td class="tg-0pky">scielo</td>
    <td class="tg-0pky">89.66</td>
    <td class="tg-0pky">88.77</td>
    <td class="tg-0pky">89.07</td>
    <td class="tg-0pky">89.17</td>
  </tr>
  <tr>
    <td class="tg-0pky">scielo+wiki</td>
    <td class="tg-0pky">88.76</td>
    <td class="tg-0pky">88.64</td>
    <td class="tg-0pky">89.71</td>
    <td class="tg-fymr"><b>89.74</b></td>
  </tr>
</table>


## Troubleshooting

If you get Uncide/Decode Error, Please read the embedding files as a below example, by adding a new argument to specify the encoding:

<pre>
f = open(fname, "r", encoding="utf-8")
</pre> 


## Digital Object Identifier (DOI) and access to dataset files

Word Embedding models can be download from:  ??

## Citing 
Please cite our paper if you use it in your experiments or project.

<pre>
@inproceedings{soares2019medical,
  title={Medical word embeddings for Spanish: Development and evaluation},
  author={Soares, Felipe and Villegas, Marta and Gonzalez-Agirre, Aitor and Krallinger, Martin and Armengol-Estap{\'e}, Jordi},
  booktitle={Proceedings of the 2nd Clinical Natural Language Processing Workshop},
  pages={124--133},
  year={2019}
}
</pre>


## Contact

Siamak Barzegar (siamak.barzegar@bsc.es)


## License

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

Copyright (c) 2018 Secretaría de Estado para el Avance Digital (SEAD)
