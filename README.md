# 🧑‍🏫 RateMyProfessor - Word Gender in Comment
This repository is the final project for STATS 201.

## 📝 Project information
- Team members: Hanyang Zhou, Raodan Zhang, Lexi Zhou
- Instructor: Markus Neumann

## 🔍 Research question
***How does gender influence students’ comment on college professors of similar teaching level in the same discipline?***

We plan to investigate the dataset gathered from the largest online rating website for college professors in the U.S., Canada, and the United Kingdom: Rate My Professor (RMP). The platform basically sets a profile for every professor in different colleges with their disciplines, and enables students to rate the professor from scale 1-5, while leaving some comments in texts. In our study, we plan to find the association between professors, who are of the same discipline and similar educational level, and the most frequently appeared words in their comment texts, and compare the differences. In this case, we want to understand how female and male professors are perceived differently in students. Are there any specific expectations, criticisms, or praises more frequently found in comments of the professor of one gender but not the other? Further, by addressing these questions, we hope to extend the understanding of female and male professors in academia and hopefully bridge any possible biases between the two groups for a more gender equal environment.

## 📍 Navigation Instructions
This repository is structured into several directiories to support running the code, accessing datasets, and reviewing documentation. The following section explains how to navigate the repository.
### 1. Code for ML Tasks
- Location: /Code
- This folder contains Jupyter Notebooks for data preparing.
  - W2_1. Data Preparation.ipynb - Merge datasets and cleans the dataset.
  - W2_2. Gender label.ipynb - Label gender of professors based on student comments.
  - W3_1. Revise rating groups - Revise rating scales to the official RMP scales (Good: 3.5-5, Average: 2.5-3.4, Poor: 1-2.4).
  - W3_3. Gender_Check_Finalized - Assign all comments with a concrete gender.
  - W3_4. Baseline.ipynb - Train/test split and baseline models with justification.
  - W4_1. Feature Engineering - Extended data preprocessing and experimental grouping structure split.
 
### 2. Datasets
- Location: /Data
- This folder contains the datasets applied in this research. Key datasets include:
  - RMP_merged.csv - The raw dataset of all RateMyProfessor entries
  - 09_RMP_prof_gender_manual_updated.csv - The prepared dataset with professor-level labeled entries after manual checking.
  - 11_RMP_prof_gender_preprocessed.csv - Comment columns with text preprocessed.
  - 16_RMP_gender_finalized.csv - With concrete gender for every comment text.
  - 17_RMP_comment_department.csv - Comments with further preprocessed text, departments classified.
  - 18_RMP_humanities/stem_poor/average/good.csv - 6 subgroups for modeling.

### 3. Visualizatuion
- Location: /Visualization
- This folder contains figures generated in this research.

## 📊 Data Preprocessing
- 1. Gender classification:
  - 1.1 Labeled each comment based on the presence of gendered pronouns (e.g., he, she, his, her).
  - 1.2 Aggregated comment-level gender labels to the professor level and manually reviewed inconsistent or conflicting cases.
  - 1.3 Applied existing name–gender reference datasets to label remaining unknown entries when pronoun-based signals were absent.
- 2. Comment text preprocessing: Performed preprocessing at the comment level to generate the final modeling input (comments_final), including:
  - basic text cleaning (escape characters and whitespace), lowercasing, punctuation removal, contraction and negation expansion, tokenization, lemmatization (with fallback), negation marking, n-gram construction (1–3 grams), and stopword removal while retaining negation terms.
    
## 📦 Baseline Modeling
- 1. Train/test split: Split the dataset into training and test sets using an 80/20 random split with a fixed random state to ensure reproducibility.
- 2. Gender-agnostic baseline: Overall high-frequency word stability (train/test Jaccard analysis).
- 3. Gender-aware baseline: Gender-differentiated word usage (Difference-of-Proportions & DAR).

## 🖊️ Acknowledgement
### Division of Responsibilities
For this project stage, each group member was responsible for the following components:
- Raodan Zhang:
    - Background research
    - Dataset external gender validate
- Hanyang Zhou:
    - Data cleaning and labeling by gender inferred from text。
    - Revise rating scale and conduct text preprocessing; Split train/test dataset;

- Lexi Zhou:
  - Descriptive dataset analysis.
  - Develop baseline models to assess word frequency patterns and preliminary gender differences in student comments.
  - Designed text features and representations, and set up the controlled experimental structure for later model and lexical comparisons.

 
