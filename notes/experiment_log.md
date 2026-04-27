# Experiment Log

## Day 1 – Setup & Initial Exploration

### What I did
- Set up project structure (data, notebooks, plots, reports)
- Loaded sample of dataset (100k rows) for fast exploration
- Performed initial EDA

### Observations
- Each search contains multiple hotel listings → ranking problem
- Dataset is highly imbalanced (≈2.7% bookings)
- Strong position bias (top-ranked hotels have higher booking probability)
- Many missing values, especially in competitor-related features

### Decisions
- Treat problem as ranking, not classification
- Use sampling for EDA, full dataset only for final results
- Handle missing values carefully (not all missing = bad data)
- Exclude competitor features initially when analyzing missing values

### Next step
- Deeper analysis of key features (price, competitors, clicks)
- Start feature engineering
