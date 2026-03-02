# 🧑‍🏫 Gendered Language Patterns in Professor Comments
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
- This folder contains Jupyter Notebooks for data processing and modeling.

#### Data Processing
- `W2_1_Data_Preparation.ipynb` Merge and clean datasets.
- `W3_1_Revise_rating_groups.ipynb` Recode ratings into official RMP scales.
- `W3_2_Data_preprocessing.ipynb` Standard text preprocessing.
- `W4_1_feature_engineering.ipynb` Extended text preprocessing and construction of experimental groupings.
  
#### Gender Labeling
- `W2_2_Gender_label.ipynb` Assign professor gender labels based on student comments.
- `W3_3_Gender_Check_Finalized.ipynb` Finalize gender assignment for all comments.

#### Modeling
- `W3_4_Baseline.ipynb` Baseline models.
- `W4_2_Model1_RandomForest_&_LogisticRegression.ipynb` Model1: Random Forest & Logistic Regression (RF&LR).
- `W4_3_Model2_LOR.ipynb` Model2: Log-Odds Ratio with a Dirichlet Prior (LOR).
- `W5_1_RF_LR_diagnostics.ipynb` `W5_2_LOR_diagnostics.ipynb` `W5_3_Model_revision_(extend_removal_word_list).ipynb` Two models diagnostics and revision.
- `W6_1_RF_LR_finalized.ipynb` `W6_2_LOR_finalized.ipynb` Finalized version of two models with result visualizations.

#### ▶️ How to run the code
The original notebooks were developed in Google Colab and load datasets from a shared Google Drive. To run the code locally or after cloning this repository, please download the required CSV files from the `/Data1` folder and update the file paths accordingly. Specifically, remove or comment out the Google Drive mounting code, and replace the original `FILE_PATH` or `base_path` with the local path to the downloaded data files. No other code changes are required.
 
### 2. Datasets
- Location: /Data1
- This folder contains the datasets applied in this research. Key datasets include:
  - `RMP_merged.csv` The raw dataset of all RateMyProfessor entries.
  - `16_RMP_gender_finalized.csv` With concrete gender for every comment text.
  - `17_RMP_comment_department.csv` Comments with preprocessed text, departments classified.
  - `18_RMP_humanities/stem_poor/average/good.csv` 6 subgroups for modeling.

### 3. Visualizatuion
- Location: /Visualization
- This folder contains figures generated in this research.
  - `W2_2.Gender label_distribution.png` Distribution of professor gender labels in the dataset.
  - `W3_4. Baseline_top_gender_words.png` Top gender-leaning words identified by the baseline.
  - `W4_Z-scores_comparison_scrubbed.png` Heatmap of z-scores comparison.
  - `W5_LOR_revised model_NEG+Uni_heatmap & Frequency-weight plot.png` LOR diagnostics.
  - `W6_(model)_word cloud_unigram & bigram.png` Word cloud visualization.
  - `W6_(model)_diverging_bar_charts_unigram & bigram.png` Diverging bar charts visualization.

## 📊 Data Preprocessing
- 1. Gender classification:
  - 1.1 Labeled each comment based on the presence of gendered pronouns (e.g., he, she, his, her).
  - 1.2 Aggregated comment-level gender labels to the professor level and manually reviewed inconsistent or conflicting cases.
  - 1.3 Applied existing name–gender reference datasets to label remaining unknown entries when pronoun-based signals were absent.
- 2. Comment text preprocessing: Performed preprocessing at the comment level to generate the final modeling input (comments_final), including:
  - basic text cleaning (escape characters and whitespace), lowercasing, punctuation removal, contraction and negation expansion, lemmatization, negation prefix marking, n-gram construction, and stopword removal while retaining negation terms.
    
## 📦 Baseline Modeling
- 1. Train/test split: Split the dataset into training and test sets using an 80/20 random split with a fixed random state to ensure reproducibility.
- 2. Gender-agnostic baseline: Overall high-frequency word stability (train/test Jaccard analysis).
- 3. Gender-aware baseline: Gender-differentiated word usage (Difference-of-Proportions & DAR).
 

## 🔧 Modeling
### Model 1: Random Forest with Logistic Regressiosn
We use the feature importance of Random Forest and assign corresponding gender prediction directions by Logistic Regression.

#### Why use Random Forest and Logistic Regression?
- It's a typical approach to deal with complex, non-linear data and extract importance words in texts for prediction.
- It shows direct metrics, coefficient and feature importance to quantify model performances.

#### How the Model Works？
- 1. TfidVectorization：We turn the comments into TF-IDF vectors for later analysis.
  2. Model Training: We train the Random Forest & Logistic Regression with train/test split and evaluate their comparability and performance with metrics (e.g., oob score, accuracy).
  3. Model Combination: Since feature importance in RF model doesn't have gender prediction directions, we introduce LR, extract its coefficient. Based on the coefficient's sign, we can assign the gender direction to feature importance (given the LR & RF also showed similiar metrics in prediction) and output a table with top 30 words, corresponding feature importance (RF), coefficient (LR), and predicted gender.
 
#### Key Findings
Although lots of words are not consistently important across subsets in top30 important results, it turned out that the word "professor" is most predictable of male professors, while nice is most predicatble of female professors.


### Model 2: Log-Odds Ratio with a Dirichlet Prior
We use the Log-Odds Ratio with a Dirichlet Prior (Monroe et al., 2008) to analyze gendered language in student comments. LOR focuses on interpreting bias with statistical confidence.

#### Why use LOR?
- It prevents high-frequency stop words (like the, and) from dominating the results。
- It  uses Bayesian Shrinkage to stop rare words (appearing only 1-2 times) from creating false extreme results.
- It allows us to compare how the same word behaves differently in STEM versus Humanities.

#### How the Model Works？
- 1. Word Counting: We count word frequencies for female and male professor comments across 6 subsets (STEM/Humanities x Poor/Avg/Good).
- 2. Dirichlet Prior: We incorporate a Background Corpus as a prior. This smooths the data and handling cases where a word count is zero.
- 3. Log-Odds Calculation: We calculate the ratio of a word's usage between groups on a symmetric logarithmic scale.
- 4. Variance Estimation: We calculate the Uncertainty for each word. Rare words receive a variance penalty, meaning the model trusts them less.
- 5. Z-score Normalization: The final score represents the Confidence of the bias.
  - Positive Z-score: Male-leaning (Red in our heatmaps).
  - Negative Z-score: Female-leaning (Blue in our heatmaps).

#### Key Findings
Our model reveals that students often focus on a female professor's personality (e.g., sweet, nice) while emphasizing a male professor's professional status (e.g., professor).

## 🔧 Model diagnostics and revision
### Model 1: Random Forest + Logistic Regression (RF&LR)
We checked the confusion matrix, ROC-AUC, and tried to find the best parameter for RF and LR.

#### 1. Model diagnostics and sensitivity checks
- Our heatmaps of RF&LR in different conditions showed that function words are still prevelant in the final result, and NEG_prefix + Unigram token condition is most interpretable among all conditions.
- Meanhwile, the accuracy and ROC-AUC of the RF&LR model in different token conditions are all around 60%, indicating space for improvement.

#### 2. Model revision: remove english stopwords and hyperparameter tuning by GridSearchCV
- Learning from the metrics, we decided to remove 'english' stopwords and use hyperparameter to improve the performance of RF&LR model.
- The hyperparameter tuning was set by GridSearchCV, with 70% focus on balancing RF&LR, so that two models can complement each other, and the other 30% focus on performance.
- However, except for f1 score, both accuracy and ROC-AUC rate decreases for the updated strategy compared to the previous one, and the confusion matrix didn't report significantly more accurate gender predictions. Thus, we chose not to use the hyperparameter strategy, and only remove 'english' stopwords. Also, we chose NEG_prefix + Unigram based on diagnosis.

#### 4. Final model:
- Our final model implements a NEG_prefix + Unigram approach on the 'english' stopwords removed dataset.

### Model 2: Log-Odds Ration (LOR)
We validated and refined our Log-Odds Ratio (LOR) model using the Monroe et al. (2008) framework.
#### 1. Model diagnostics.
- We used Frequency-weight plots to check if the model is misled by high-frequency or rare words.
- Our plot confirms that rare words were successfully pulled toward the baseline (z=0).
- For high-frequency terms, most are concentrated in the middle-frequency zone and those high ones were cross-checked via heatmaps, showing consistent gendered directions across all disciplines.

#### 2. Sensitivity checks
- - Prior comparison: We compared informed Dirichlet Priors using a background prior aganist flat priors.
  - The results show high robustness, as 13 out of the top 20 words remain identical across all configurations.

#### 3. Model revision: remove discipline specific terms
Initial results contained discipline nouns that reflected department distribution rather than student perception. We revised the model through a systematic filtering process:
- Collected all unique department names => broke into individual tokens => manual review (keep general academic terms) => applied the blacklist to the comment_scrubbed column and finalize the comment column.

#### 4. Final model:
- Our final model implements a NEG_prefix + Unigram approach on the cleaned dataset.


## 📋 Interpretation of results
### 1. Random Forest + Logistic Regression
Across all subsets with unigram processing, “teacher” indicates female professors, while “professor” predicts male professors. Obviously, teacher is the title inferior to professor. The reference difference is also prominent in bigram processing, where female professors are also frequently referred to as “person” (e.g., nice person) besides “teacher”. Although some exemptions exist, such as in Poor-rated humanities, female professors were also referred as “worst professor”, the general title reference difference is alert to us and imply an underestimation of female professor’s authority. Another big difference is the expectations of personality traits for female and male professors. While affective words are generally more predictable to female professors, male professors are uniquely expected to be funny, especially in humanities.

### 2. Log odds ratio
By combining the unigram and bigram analyses, our research suggests two consistent language patterns of professor evaluation by gender. 

First, the standards for female professors are highly associated with the personal nature of caring and nurturing. For the positive comments, both unigram and bigram analysis show similar patterns related to personality, including nice, sweet, helpful, very sweet, super nice, and a nice person. When the students give negative ratings on female professors, they tend to use words such as unorganized, mad, or very strict. These comments directly contrast with the positive comments about female professors’ caring personalities. This further demonstrates that students’ expectation of female professors is related to their nurturing personality.

Second, the standards for male professors are related to 1) professionality (e.g., professor and smart) and 2) entertaining characteristics. Male professors are commented on by phrases like very funny, sense humor, and learn a lot. This indicates that male professors are associated with expectations on their entertaining personality and professional performance. 

  
## 🖊️ Acknowledgement
### Division of Responsibilities
For this project stage, each group member was responsible for the following components:
- Raodan Zhang:
    - Background research
    - Dataset external gender validate
    - Random Forest and Losgistic Regression model set, diagnosis, and revision.
    - Interpreted and analyzed the language patterns across discipline/rating subsets of the Random Forest + Logistic Regression model.
      
- Hanyang Zhou:
    - Data cleaning and labeling by gender inferred from text。
    - Revise rating scale and conduct text preprocessing; Split train/test dataset;
    - Conducted the Log-Odds Ratio with a Dirichlet Prior to analyze student comments.
    - Extended the removal-word list by adding discipline specific terms. Conducted Log odds ratio (LOR) model revision.
    - Interpreted and analyzed the language patterns across discipline/rating subsets of the LOR model.

- Lexi Zhou:
  - Descriptive dataset analysis.
  - Develop baseline models to assess word frequency patterns and preliminary gender differences in student comments.
  - Designed text features and representations, and set up the controlled experimental structure for later model and lexical comparisons.
  - Adapted the original LOR and RF+LR pipelines, added alternative text-processing configurations, and built the diagnostics used for error analysis and robustness checking.
  - Finalized the code; Generated result visualizations; Proofread the report.

 
