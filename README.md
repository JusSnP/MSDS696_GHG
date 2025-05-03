# Modeling Scope 3 GHG Emissions, Part 2 
### MSDS 696 S8W2 2025 - Regis University

*Please visit my [MSDS 692](https://github.com/JusSnP/MSDS696_GHG) repo for the first part of this research project.*

### Use-case

Modeling scope three greenhosue gas emissions based on data containing nominal, financial, economic, and emissions reporting features.

For more details please see my [final presentation](/Assets/GHG_MSDS696.pptx) and the [live recording](https://github.com/Regis-University-Data-Science/Practicum-Showcase).
 
### Summary  
This repository contains notebooks, data, and a python library associated with work performed during MSDS 692 - Data Science Practicum 1 as a member of Dr. Sorauf's Greenhouse Gas Emissions research group. For the second half of my data science practicum I focused on refining simple feed-forward neural networks to generalize for best results across all of the 17 scope three greenhouse gas emission types. However, I did begin the term continuing work with the ESG data acqurired during MSDS 692. While I was able to create relatively well performing models for _some_ emissions types there is still a large amount of work left to be completed to produce models that could truly be deployed to the commercial sector. 

### Contents

* Assets - Assorted files: This directory contains my final presenation and any images used within this readme.
* Code - Two notebooks
  While this project used more than 10 notebooks the notebooks contained in this directory contains the most polished and complete of all versions. Differences between the notebooks are either variations in network topology/hyperparameters, which is provided below, or in implementing coding/data science best practices.
    * nn_data_prep_V2 - Jupyter notebook: This notebook prepares and visualizes the source data for use in the feed-forward neural network notebook. Note EDA, cleansing, and outlier treatment was performed the previous term by a different team member. 
    * nn_v14 - Jupyter notebook: This notebook contains the 'pipeline' for training and evaluating the final model. 
* Data - One CSV and one sub-directory containing 17 additional CSVs 
    * GHG_Post_Outlier - CSV file: This is the source data which is processed by nn_data_prep_V2.
    * Scope_3_emissions_type_csvs/ - Sub-directory: This directory contains 17 CSVs separating the source data by type three emissions type. 
* logs - 364 total directories containing log files
    * (Scope_3_emissions_type) - Sub-directory: This directory contains sub-directories holding training and validation data for all folds completed during training on all type three emissions types.
    * fit - Sub-irectory - This directory contains timestamped training and validation data for use with TensorBoards.
* Models - Keras files: This directory contains stored models for all neural networks trained (v2-v14).
* Results - 13 CSVs: The CSVs contained within this directory hold metrics of model performance for each scope three emissions type organized by neural network version 

### Discussion

_This is a non-comprehensive summary of the work completed during MSDS 696._

**Data**

My first step was to integrate the data obtained during the first half of my Data Science Practicum from [Financial Modeling Prep](https://site.financialmodelingprep.com/) with the output data from [another team member's work](https://github.com/julieanneco/Scope3_Emissions) on outliers, skewness, and cleansing of our original "mega merged" file of 180k+ samples. [Econimic Social and Governance](https://greenly.earth/en-us/blog/company-guide/what-is-esg-data-and-how-to-use-it) data, was first merged on the feature **account_name** into the pre-treated data. The total dataset contained 40k+ samples and 25+ features. 

It was discovered that the feature account_name could have duplicity in association with accound_id. To rectify account_names, the most recently reported account_name was chosen to replace all account_names associated with a single account_id. This method was also used to correct duplicity found in stock symbol related features. 

After correcting account_id and stock symbol features, remaining zeros found within the data were treated. It was determined that imputing would not be perfomed on the dataset and instead all samples with NaN or 0 entries would be dropped. This was due to the fact that features within the data have high skewness, range, and other nuance. Imputing in a meaningful way may not be possible. The image below shows the remaining samples by emissions type after dropping NaNs/0s.

![Samples after dropping zero and NaN rows](/assets/counts_with_ESG.png)








