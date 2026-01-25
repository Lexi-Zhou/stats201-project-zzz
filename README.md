# RateMyProfessor - Word Gender in Comment
This repository is the final project for STATS 201.

## Project information
- Team members: Hanyang Zhou, Raodan Zhang, Lexi Zhou
- Instructor: Markus Neumann

## Research question
***How does gender influence students’ comment on college professors of similar teaching level in the same discipline?***

We plan to investigate the dataset gathered from the largest online rating website for college professors in the U.S., Canada, and the United Kingdom: Rate My Professor (RMP). The platform basically sets a profile for every professor in different colleges with their disciplines, and enables students to rate the professor from scale 1-5, while leaving some comments in texts. In our study, we plan to find the association between professors, who are of the same discipline and similar educational level, and the most frequently appeared words in their comment texts, and compare the differences. In this case, we want to understand how female and male professors are perceived differently in students. Are there any specific expectations, criticisms, or praises more frequently found in comments of the professor of one gender but not the other? Further, by addressing these questions, we hope to extend the understanding of female and male professors in academia and hopefully bridge any possible biases between the two groups for a more gender equal environment.

## Navigation Instructions
This repository is structured into several directiories to support running the code, accessing datasets, and reviewing documentation. The following section explains how to navigate the repository.
### 1. Code for ML Tasks
- Location: /Code
- This folder contains Jupyter Notebooks for data preparing.
  - W2_1. Data Preparation.ipynb - Merge datasets and cleans the dataset.
  - W2_2. Gender label.ipynb - Label gender of professors based on student comments.
 
### 2. Datasets
- Location: /Data
- This folder contains the datasets applied in this research. Key datasets include:
  - RMP_merged.csv - The raw dataset of all RateMyProfessor entries
  - 09_RMP_prof_gender_manual_updated.csv - The prepared dataset with professor-level labeled entries after manual checking.

### 3. Visualizatuion
- Location: /Visualization
- This folder contains figures generated in this research.


## Acknowledgement
### Division of Responsibilities
For this project stage, each group member was responsible for the following components:
- Raodan Zhang:
  - Primary responsibility:
    - Background Research
  - Specific tasks completed in week 2:
    - Clarified the scope and background of the research question
    - Envision future data analysis approaches
- Hanyang Zhou:
  - Primary responsibility:
    - Prepare and process datasets. 
  - Specific tasks completed in week 2:
    - Data cleaning and labeling by gender inferred from text.
    - Manual review for conflict cases.
    - Record data prepration memos.

- Lexi Zhou:
  - Primary responsibility: descriptive dataset analysis
  - Specific tasks completed in week 2:
    - Defined dataset variables and their meanings.
    - Performed descriptive analysis of student rating groups.
    - Summarized professor gender distribution based on existing labels.

