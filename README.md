# Modeling Scope 3 GHG Emissions, Part 2 
### MSDS 696 S8W2 2025 - Regis University

*Please visit my [MSDS 692](https://github.com/JusSnP/MSDS696_GHG) repo for the first part of this research project.*

 
### Summary  

For more details please see my [final presentation](/Assets/GHG_MSDS696.pptx) and the [live recording](https://github.com/Regis-University-Data-Science/Practicum-Showcase).

### Contents

*Assets
* Code - Two notebooks
  While this project used more than 10 notebooks the notebook contained in this directory contains the most polished and complete of all versions. Differences between the notebooks are either variations in network topology or hyperparameters, which is provided below, or in implementing coding/data science best practices.
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

### Methods



### Discussion

This is a non-comprehensive summary of the work completed during MSDS 692.

The API used to call FMP data was updated since the reserach group's original pull which required some updating. Additionally there were issues with how the data was being sorted, organized, and merged through the API call libraries. There were consistent issues with what is assumed to be server-side timing issues producing inconsistent numbers and composition of data retrieved from FMP.  

![comparison of new and old historical employee count data pulls](/images/EC_discrepancy.png)  

EDA and light feature engineering was conducted on the new data. It was found that the majority of companies represented within the newly acquired features are US corporations, or traded on US stock exchanges.

![company makeup of ESG data](/images/ESG_countries.png)  

Covariances of Scope_3_emission_type with all other features was calculated for each of the 15 unique emissions types contained within our data. It was discovered that, indeed, each emission type had unique covariances and they should be treated individually as they during outlier treatment being conducted by other team members.  

![covariance for downstream leased assets](/images/covariance.png)  
![covariance for downstream transportation and distribution](/images/covariance2.png)  

Finally, the teams original XGBoost model was re-run using the newly acquired 10 year's worth of data *without new FMP data*. This was then compared to a simple feed-forward neural network as a baseline for future work and a determining factor if neural networks could be leveraged for this work.  

![Neural network loss curves](/images/nn.png)  

![XGBoost MAE](/images/XGBoost.png) 





