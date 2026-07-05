This notebook builds a complete workflow for predicting the 2026 FIFA World Cup group-stage matches and simulating group standings using historical match data and FIFA ranking data.

## What the notebook covers

### 1. Setup and data loading
- Imports the main Python libraries used for data processing, modeling, and visualization.
- Defines a helper function to load CSV files from the project folder.
- Loads the key datasets:
  - results.csv
  - group_fixtures.csv
  - knockout_slots.csv
  - fifa_ranking-2023-07-20.csv
  - fifa_ranking-2024-04-04.csv
  - fifa_ranking-2024-06-20.csv

### 2. Data preparation and cleaning
- Parses date columns so they can be used for time-based joins.
- Standardizes team names with a manual mapping so the same teams are represented consistently across files.
- Checks how well the fixture teams match the historical results dataset.

### 3. Building the training dataset
- Combines the FIFA ranking snapshots into one dataset.
- Sorts historical results and ranking data by date.
- Merges home-team and away-team rankings into the match results using date-based joins.
- Filters the historical dataset to competitive tournaments only.
- Removes rows missing the key columns needed for modeling.
- Keeps only matches from 2010 onward to create a more relevant training set.

### 4. Feature engineering from team form
- Converts each match into two team-side rows: one for the home team and one for the away team.
- Creates outcome fields such as goal difference, win, draw, and loss.
- Calculates cumulative historical stats for each team before each match.
- Builds prior-performance features such as:
  - average goals for
  - average goals against
  - average goal difference
  - average points per game
  - win/draw/loss rates
- Converts these team-level features back into match-level features for training.

### 5. Model training inputs
- Creates the final feature matrix using:
  - home and away ranks
  - rank difference
  - prior form metrics for both teams
- Defines the target variables:
  - home goals
  - away goals
  - goal difference
  - match result category
- Removes rows without prior-history features and checks the distribution of the targets.

### 6. Predictive modeling
- Trains a Poisson regression model to predict expected goals for both teams.
- Uses 5-fold cross-validation to evaluate the goal predictions.
- Trains a Random Forest Classifier to predict match outcomes: home win, draw, or away win.
- Evaluates the classifier using accuracy, macro F1 score, confusion matrix, and feature importance.

### 7. Predicting upcoming group fixtures
- Uses the latest available team form and latest FIFA rankings to create feature rows for each group-stage fixture.
- Predicts:
  - expected home and away goals
  - rounded predicted scores
  - outcome probabilities for home win, draw, and away win
  - the most likely result

### 8. Simulating group standings
- Converts the predicted match results into group standings.
- Calculates points, goal difference, goals for, and goals against for each team.
- Ranks teams within each group and marks the top two as qualifiers.

### 9. Exporting results
- Writes the predicted fixture results to group_predictions.csv.
- Writes the simulated group tables to group_standings.csv.

## End goal
The notebook turns historical football data into a practical forecasting pipeline that estimates match scores, predicts match outcomes, and simulates how the World Cup groups might unfold.
