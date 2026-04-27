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
