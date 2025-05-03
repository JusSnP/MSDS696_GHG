# Modeling Scope 3 GHG Emissions, Part 2 
### MSDS 696 S8W2 2025 - Regis University

*Please visit my [MSDS 692](https://github.com/JusSnP/MSDS696_GHG) repo for the first part of this research project.*

 
### Summary  
This repository contains notebooks, data, and a python library associated with work performed during MSDS 692 - Data Science Practicum 1 as a member of Dr. Sorauf's Greenhouse Gas Emissions research group. For the second half of my data science practicum I focused on refining simple feed-forward neural networks to generalize for best results across all of the 17 scope three greenhouse gas emission types. However, I did begin the term continuing work with the ESG data acqurired during MSDS 692. While I was able to create relatively well performing models for _some_ emissions types there is still a large amount of work left to be completed to produce models that could truly be deployed to the commercial sector. 

For more details please see my [final presentation](/Assets/GHG_MSDS696.pptx) and the [live recording](https://github.com/Regis-University-Data-Science/Practicum-Showcase).

### Contents

* Assets - Assorted files: This directory contains my final presenation and any images used within this readme.
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

### Discussion

_This is a non-comprehensive summary of the work completed during MSDS 696._

fdsfds








