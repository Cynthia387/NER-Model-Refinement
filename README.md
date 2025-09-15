# Named Entity Recognition (NER) Model Pipeline

## Overview
This pipeline is designed to identify and categorize named entities in text using a custom NER model. It aims to extract entities such as people (PER), organizations (ORG), and locations (LOC).

---

## Pipeline Structure
1. **Load the Pre-trained Model**
   - Import packages needed (e.g. spacy, pandas).
   - Import input articles.

2. **Remove In-text Hyperlinks**
   - Iterate through sentences, detect and delete all-capitalized sentence embedded in main content.
  
3. **Enable Entity Linker**
   - Connecting to Wikidata knowledge base, categorize similar calling of one interest group, and label the primary name to the column called "Entity".

4. **Run the Model and Keep Results**
   - Idenitify entities, list in column called "Pattern", categorized to Names, Organizations, and Locations.
   - Append the entity and its corresponding ID of the article, publication data, and article URL, to a new dataframe.

5. **Write Output to New File**
   - Write the dataframe to a created CSV file as the result.

---

## Files
- `spacy-entity-linker.ipynb`: Contains the pipeline implementation, training, evaluation, and analysis workflows.
- `emission_extracted_entities_org_linker.csv`: Directory for the resulting output file.

---

## Installation
1. Clone the repository:
   ```bash
   git clone <https://github.com/infoqualitylab/NER-Model>
   cd <repository-folder>
   ```
2. Install dependencies:
   ```bash
   pip install spacy
   pip install pandas
   pip install spacy_entity_linker
   ```
3. Prepare your list of news articles called "test_articles.csv", your list of commenters of interest called "patterns.csv".

4. Ensure that the required datasets are available in the same local directory.

---

## NER Pipeline – Results

The pipeline exhibited two main types of validation errors:
- **Highlighted in annotation, but pipeline did not recognize**  
- **Recognized by pipeline, but not highlighted in annotation**

### Summary of Unique Interest Groups

| Entity Type | Recognized by Pipeline | Annotated Manually |
|:-----------:|:----------------------:|:------------------:|
| ORG         | 345                    | 309                |

### Confusion Matrix (Interest Groups)

| Annotated w/ Doccano \ Recognized by Pipeline | No                          | Yes                            |
|:---------------------------------------------:|:---------------------------:|:------------------------------:|
| **No**                                        | 133 (Not labeled with ORG)  | 34 (Annotated Not Recognized)  |
| **Yes**                                       | 50 (Not Annotated Recognized) | 275 (Annotated Recognized)     |

---

## Limitations
- The pipeline identifies extra “organizations”. For example, I didn’t highlight “EV” which stands for “Electric Vehicle” (left). But the pipeline recognizes it (right).
- Not all interest groups are linked. For instance, “Alliance for Automotive Innovation” is not linked because it is missing from WikiData, which SpaCy checks for Entity Linking.
- Our pipeline only supports English. Adaptation for multiple languages would be possible using more SpaCy features.
- Manual validation of outputs introduces potential for human error, impacting the reliability of descriptive statistics.

---

## Future Improvements
- In the future, will use Media Bias/Fact Check’s ratings to identify news outlets often read by Republicans vs. Democrats so that we can compare which interest groups appear.
- We will look for names of individuals in news outlets using NER for Person. We could then match them with individuals who commented on emissions standards.
- Experiment with different architectures and embeddings.

---

## Contributors
- [Xiaoran Zhou]

# Pipeline Refinement: Handling leading "the" in entities

## Overview

This project refines the existing NER + entity linking pipeline (originally developed by Xiaoran Zhou using spaCy and the spacy-entity-linker package).
Two contributions were made:

1. **Small Dataset Validation**
   - File: revise_check.ipynb
   - Tested whether removing the leading article “The” from organization names improves linking accuracy, using a dataset of 192 unlinked entities (entities_duplicates_removed0709.csv).

2. **Full Pipeline Integration**
   - File: spacy-entity-linker(revised)_copy.ipynb
   - Added a new cell to the original large dataset pipeline (emission_rule_news_steven.csv) that re-runs linking on previously unlinked entities after stripping leading “The”.
   The baseline pipeline from Xiaoran Zhou was preserved.

---

## Pipeline Structure

1. NER step: Extract organization entities (ORG) from article text using spaCy.

2. Entity linking step: Pass recognized entities into spacy-entity-linker, which matches them to Wikidata entries.

3. Refinement step (new):
   Case-insensitive removal of leading “The”.
   Retry linking only for rows where Entity == None.
   Save results in a new column (Entity_TheFix) alongside baseline output.

---

## Results

1. Small Dataset (192 unlinked entities)

Originally unlinked: 192
Linked after “The” removal: 42
Still unlinked: 150

~21.9% of previously unlinked entities were recovered.

2. Full Dataset (~18k mentions)

Baseline unlinked: 17,970
After “The” fix: 13,483
Newly linked: 4,487

~25% recovery rate among unlinked entities.

---

## Notes

- Refinement applies only at the entity linking stage, not during NER.
- Future work will extend to handling abbreviations, encoding issues and so on (see technical report).

---

## Contributor
- [Xiuping Shen]

