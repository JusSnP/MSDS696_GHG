# Modeling Scope 3 GHG Emissions, Part 2 
### MSDS 696 S8W2 2025 - Regis University

*Please visit my [MSDS 692](https://github.com/JusSnP/MSDS692_GHG) repo for the first part of this research project.*

### Use-case

Modeling scope three greenhosue gas emissions based on data containing nominal, financial, economic, and emissions reporting features.

For more details please see my [final presentation](/Assets/GHG_MSDS696.pptx) and the [live recording](https://github.com/Regis-University-Data-Science/Practicum-Showcase).
 
### Summary  
This repository contains notebooks, data, and a python library associated with work performed during MSDS 692 - Data Science Practicum 1 as a member of Dr. Sorauf's Greenhouse Gas Emissions research group. For the second half of my data science practicum I focused on refining simple feed-forward neural networks to generalize for best results across all of the 17 scope three greenhouse gas emission types. However, I did begin the term continuing work with the ESG data acqurired during MSDS 692. While I was able to create relatively well performing models for _some_ emissions types there is still a large amount of work left to be completed to produce models that could truly be deployed to the commercial sector. 

### Contents

* Assets - Assorted files: This directory contains my final presenation and any images used within this readme.
* Code - Two notebooks  
  While this project used more than 10 notebooks the notebooks contained in this directory contains the most polished and complete of all versions. Differences between the notebooks are either variations in network topology/hyperparameters, which is provided below, or in implementing coding/data science best practices. I _think_ the code has clear marking, but if there are any questions please reach out!
    * nn_data_prep_V2 - Jupyter notebook: This notebook prepares and visualizes the source data for use in the feed-forward neural network notebook. Note EDA, cleansing, and outlier treatment was performed the previous term by a different team member. 
    * nn_v14 - Jupyter notebook: This notebook contains the 'pipeline' for training and evaluating the final model.
>[!NOTE]
>The notebooks in this repo contain environment specific lines which must be edited as appropriate before running.
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

My first step was to integrate the data obtained during the first half of my Data Science Practicum from [Financial Modeling Prep](https://site.financialmodelingprep.com/) with the output data from [another team member's work](https://github.com/julieanneco/Scope3_Emissions) on outliers, skewness, and cleansing of our original "mega merged" file of 180k+ samples. [Economic, Social, and Governance](https://greenly.earth/en-us/blog/company-guide/what-is-esg-data-and-how-to-use-it) data, was first merged on the feature **account_name** into the pre-treated data. The total dataset contained 40k+ samples and 25+ features. 

It was discovered that the feature account_name could have duplicity in association with accound_id. To rectify account_names, the most recently reported account_name was chosen to replace all account_names associated with a single account_id. This method was also used to correct duplicity found in stock symbol related features. 

After correcting account_id and stock symbol features, remaining zeros found within the data were treated. It was determined that imputing would not be perfomed on the dataset and instead all samples with NaN or 0 entries would be dropped. This was due to the fact that features within the data have high skewness, range, and other nuance. Imputing in a meaningful way may not be possible. The image below shows the remaining samples by emissions type after dropping NaNs/0s.

![Samples after dropping zero and NaN rows](/Assets/counts_with_ESG.png)

Noting that many of the emissions types have single to low double digit sample counts it was determined that ESG data would be abandoned and the remainder of the project would be conducted on the 40k sample dataset mentioned above without merging in ESG data. Below shows samples per emission type without the drops induced by adding ESG data. Notice there are still many emissions types with low sample sizes, but the Y-scale has shifted significantly upward.

![Samples without adding ESG](/Assets/counts_withoutESG.png)

The data was then split into 17 separate dataframes (CSVs) by emission type. Previous analysis showed extreme ranges within the data (5+ orders of magnitude) for many features. To overcome this and prepare for model training standard scaler was implemented within the model training loops. The only other data treatment conducted was performing one-hot encoding on the categorical features _which was done outside of the training loops and is now recognized as a potential source for data leakage_.

**Modeling**

Multiple caveats were established prior to developing neural networks.

1) This project would focus on feed-forward neural networks and no other architectures.
2) One "optimal network" would be selected understanding that each scope three emissions type most likely needs its own topology and hyperparameters for best results.
3) Manual selection of hyperparamters would be conducted since grid search attempts proved to extend well into the day(s) to run timeframe and limited troubleshooting/analysis opportunities
4) There's going to be plenty of work to do after this...

Multiple iterations of topology and hyperparameters were trained on each scope three emissions type and were denoted v2-v14. The image below contains a description of each network. The networks were built using the Keras library contained within the TensorFlow library in order to exploit TensorBoards as a side quest. 
 
![Network iteration details](/Assets/networks.png)
 
Each iteration of the network architecture was initially only judged based on [R<sup>2</sup>](https://statisticsbyjim.com/glossary/r-squared/). This metric explains how well a regression model fits to the data with acknowledged deficiencies in cases such as bias within the data. A single metric was chosen for simplicity while for more thorough treatments multiple metrics should be compared in order to determine if the model works best _for the specific use case and data_. Ultimately the configuration found in v10 was chosen as optimal for all emission types as a whole and K-Folds and early stopping where added to the code. The image below shows the model performance for each emission type. Note v10 is not always the best, it just trended higher for more emission types on average.

![Model performance](/Assets/network_performance.png)

While iterating on the model configurations it was discovered that there were data leakage issues and incorrect implementation of K-Means when initializing the final model with average weights. Ultimately these issues were (mostly) fixed and implemented in v14 which was a simple feed-forward neural network with two hidden layers, dropouts, regularization, and other paramters as indiacted in the table. 

**Results**

As mentioned, it was noted that there were issues corrected in v14. One such issue, unrelated to data leakage or K-Folds implementation, was early stopping on folds. The image below shows the high variation in early stopping on folds while running v12 of the code. As understood, this indicates that there are still issues within the source data. To help mitigate this in v14 the initial test set containing all samples from 2023 was not separated out and instead train/(validation)/test split was performed on the entire dataset. 

v14 performed well for select emission types depending on which metric was examined. R<sub>2</sub> as high as 0.88 was observed while Root Mean Square Log Error (RMSLE) achieved results as low as 2.75. This project is an excellent example of the difficulty in judging a model on one metric alone. High R<sub>2</sub> _rarely_ correlated to a low RMSLE for the various emission types over all versions. The image below shows these results.

![RMSLE and R<sub>2</sub> for v14 over all emissions types](/Assets/v14rmsleR2.png)

**Future Work**

As stated, there is still alot to do before these models could be deployed to confidently predict scope three greenhouse gas emissions. Below is a non-comprehensive list of those tasks.

1) Explore alternative architectures and topologies
2) Apply grid search over all hyperparameters including activation (output) and optimizer
3) Determine optimal architecture, topology, and hyperparameters for each individual emission type
4) Re-visit outliers and distribution of source data
5) Develop a pipeline for easy deployment


