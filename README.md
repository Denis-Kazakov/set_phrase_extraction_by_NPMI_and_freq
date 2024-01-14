# Set Phrase Extraction Using Normalized Pointwise Mutural Information (NPMI)
This is a study project to extract set phrases from datasets.
## Rationale
Language translation often requires compilation of glossaries with technical terms. But there is also another entity that often requires special treatment in translation: set phrases. Some types of language, such as legalese, are particularly rich in set phrases such as "aid and abet", "in accordance with", etc. These phrases may have to be translated uniformly, which may require compiling lists of such phrases.

## Dataset and data preparation
The original dataset was downloaded from https://www.kaggle.com/datasets/bahushruth/legalclausedataset

It was intended for classification so it had two columns: one with clauses from legal contracts and the other with clause types. The clause type was also repeated in every cell of the first column before the clause text. So I dropped the first column and removed the clause type from the first column as well.

All clauses were shuffled, stripped of punctuation and any non-Latin characters, lowercased and concatenated into a single string, which was split into a list of words. 

## Algorithm
Herein, I propose an algorithm to extract set phrases by concatenating words that often occur together. It was inspired by such algorithms as byte-pair encoding and WordPiece.

The first idea to find set phrases would be to use n-gram frequency. However, the most frequent n-grams are phrases like "of the". So I used normalized pointwise mutural information (NPMI) instead. 

NPMI was originally proposed by Gerlof Bouma in _Normalized (Pointwise) Mutual Information in Collocation Extraction_. I modified the formula to enable concatenation of more than two words simultaneously with a tunable bias towards longer n-grams.

I also took into account relative frequencies of n-grams with a tunable parameter controlling relative contribution of modified NPMI and n-gram frequency.

This version is not efficient as it rebuilds frequency distributions at every pass. But I left it as is as the operation has to be done only once for every dataset.

For details, please see the notebooks.

## Files
- extractor.ipynb - the main script for set phrase extraction
- summarize_results.ipynb - creates results summary: set_phrases.xlsx with each tab holding phrases with a specific number of words 
- Tunable hyperparameters are saved in hyperparameters.json
- legal_clauses.json - the original dataset (not uploaded to this repository due to large size)
- tokens.json - same dataset with concatenated n-grams (not uploaded to this repository due to large size)
- merging_log.json holds the history of word merging into phrases
- set_phrases.json holds the list of identified set phrases