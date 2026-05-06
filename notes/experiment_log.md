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
  - Created relevance labels:
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
