# Experiment Log

## Day 1 – Setup & Initial Exploration
- Project setup
- Loaded sample of dataset (100k rows) for fast exploration
- Performed initial EDA

## First Conclusions

- Each search contains multiple hotel listings → ranking problem
- Booking rate is very low → imbalanced dataset (≈2.7% bookings)
- Strong position bias observed
- Many missing values, especially in competitor-related features
- Creation of key plots regarding this dataset

## Day 2 – Exploration and Understanding

- Switched from sample dataset to full dataset (1M+ rows) for accurate analysis
- Refined exploratory analysis and created more plots 

- Investigated missing values in depth:
  - Identified that most missing data comes from competitor-related features
  - Separated competitor and non-competitor variables for clearer analysis
  - Interpreted meaning of missing values (e.g., no user history, no search data)

- Created structured summary of the only variables with missing values (excluding the competitors variables since they are highly sparse):
  - gross_booking_usd
  - visitor_hist_starrating
  - visitor_hist_adr_usd
  - srch_query_affinity_score
  - orig_destination_distance

- Analyzed competitor data:
  - Computed number of competitors available per listing
  - Created grouped distribution (0,1,2,3,4,5+) for clarity
  - Observed high sparsity in competitor features

- Performed data validation:
  - Checked for invalid values across multiple features
  - Confirmed no data invalidation after handling missing values

## Day 3 – Data Preparation and Initial LambdaMART Model

- Created a dedicated data preparation notebook
- Prepared dataset for ranking-based learning using LambdaMART

- Handled missing values:
  - Filled `prop_location_score2` missing values with `-1`
  - Filled visitor history features with `0`
  - Added missing-value indicator flags to preserve missingness information

- Removed leakage-related features from training:
  - `gross_booking_usd`
  - `position`

- Investigated data leakage concepts:
  - Distinguished between usable features and post-booking information
  - Discussed why target variables cannot be used as input features

- Performed feature engineering:
  - Created relative price features within each search
  - Created hotel price rank inside each search query
  - Combined review score and star rating into interaction features
  - Generated normalized search-context features

- Prepared ranking groups:
  - Sorted dataset by `srch_id`
  - Grouped hotel listings by search query for LambdaMART training

- Built first LambdaMART ranking model using LightGBM:
  - Created relevance labels to act as the target variable ('y'):
    - 0 = no interaction
    - 1 = click
    - 5 = booking
  - Split train/validation sets by search query (`srch_id`)
  - Trained initial ranking model

- Evaluated model performance:
  - Computed feature importance scores
  - Identified strongest ranking features:
    - relative price
    - hotel location scores
    - review-related features
    - competitor price features

- Evaluated ranking quality using NDCG@5:
  - Achieved initial NDCG@5 score of approximately `0.38`
  - Confirmed that the model successfully learned meaningful ranking patterns
  - 
## Day 4 – Model Optimization and Hyperparameter Tuning

- Continued experimentation with the LambdaMART ranking model
- Refined target relevance definitions and evaluated their impact on ranking quality

- Tested multiple relevance configurations:
  - Booking-only relevance
  - Click + booking relevance combinations
  - Increased click relevance weights
  - Increased booking relevance weights

- Observed that graded relevance improved ranking quality:
  - Best relevance definition:
    - `0` = no interaction
    - `3` = click
    - `5` = booking
  - Confirmed that clicks provide useful intermediate ranking information while bookings remain the strongest relevance signal

- Experimented with the `position` feature:
  - Included Expedia’s original ranking position during experimentation
  - Observed a significant increase in NDCG scores
  - Determined that `position` introduces leakage-like behaviour because it reflects Expedia’s existing ranking logic
  - Removed `position` from the final model to ensure realistic evaluation and compatibility with the test dataset

- Improved competitor feature preprocessing:
  - Added missing-value flags for all competitor-related variables
  - Preserved the distinction between:
    - unavailable competitor data
    - actual competitor values (e.g., same price)

- Performed hyperparameter tuning:
  - Tested different values for:
    - `num_leaves`
    - `learning_rate`
    - `n_estimators`
  - Observed that:
    - larger tree sizes (`63`, `127` leaves) reduced performance
    - lower learning rates improved ranking stability

- Identified best-performing hyperparameters:
  - `learning_rate = 0.03`
  - `n_estimators = 700`
  - `num_leaves = 31`

- Evaluated ranking quality using NDCG@5:
  - Achieved best validation NDCG@5 score of approximately `0.3866`
  - Confirmed that hyperparameter tuning and improved competitor preprocessing slightly improved ranking performance

- Additional observations:
  - Relative price features remained among the strongest predictors
  - Competitor-related features continued to provide valuable ranking information
  - Group-based train/validation splitting by `srch_id` proved essential for realistic evaluation

