# Named Entity Recognition (NER) Model Pipeline

## Overview
This pipeline is designed to identify and categorize named entities in text using a custom NER model. It aims to extract entities such as people (PER), organizations (ORG), and locations (LOC), and includes functionality for training, evaluation, and error analysis.

---

## Pipeline Structure
1. **Load the Pre-trained Model**
   - Import packages needed (e.g. spacy, pandas).
   - Import input articles.

2. **Add an EntityRuler to the Pipeline**
   - Import the list of commenters of interest, add to the EntityRuler.

3. **Remove In-text Hyperlinks**
   - Iterate through sentences, detect and delete all-capitalized sentence embedded in main content.

4. **Run the Model and Keep Results**
   - Idenitify entities categorized to Names, Organizations, and Locations.
   - Append the entity and its corresponding ID of the article, publication data, and article URL, to a new dataframe.

5. **Write Output to New File**
   - Write the dataframe to a newly created CSV file as the result.

---

## Files
- `spacy.ipynb`: Contains the pipeline implementation, training, evaluation, and analysis workflows.
- `test_articles.csv`: Directory of dataset that stores collected articles.
- `patterns.csv`: Directory to store entities of interest.
- `extracted_entities.csv`: Directory for the resulting output file.

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
   pip install spacy.pipeline
   ```
3. Prepare your list of news articles called "test_articles.csv", your list of commenters of interest called "patterns.csv".

4. Ensure that the required datasets are available in the same local directory.

---

## Performance Summary
### Entity Identification Counts (Table 1)
| Entity Type | Identified by NER Model | Identified by Manual Annotation |
|-------------|--------------------------|----------------------------------|
| PER         | 197                      | 208                              |
| ORG         | 341                      | 339                              |
| LOC         | 201                      | 219                              |

### Error Analysis (Table 2)
| Entity Type | Entities Missed by Model | % Missed | Entities Miscategorized by Model | % Miscategorized |
|-------------|---------------------------|----------|-----------------------------------|-------------------|
| PER         | 5                         | 2.4%     | 46                                | 22.1%            |
| ORG         | 36                        | 10.6%    | 38                                | 11.2%            |
| LOC         | 10                        | 4.6%     | 8                                 | 3.7%             |

---

## Limitations
- The pipeline exhibits relatively high misclassification rates, such as 22.1% for person entities (PER), primarily due to limitations in the pre-trained model used.
- Variability in content structure across different news websites poses challenges for web scraping and content preprocessing.
- The dataset is limited to English content from a few U.S.-based news outlets, affecting the generalizability of the results.
- Manual validation of outputs introduces potential for human error, impacting the reliability of descriptive statistics.

---

## Future Improvements
- Incorporate EntityLinker with Wikidata knowledge base to sub-categorize results.
- Experiment with different architectures and embeddings.

---

## Contributors
- [Xiaoran Zhou]
- [Heng Zheng]
- [Jodi Schneider]


