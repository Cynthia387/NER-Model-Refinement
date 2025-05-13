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


