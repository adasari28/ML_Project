NBA Game Outcome Prediction
===========================

This project predicts whether the home team wins an NBA game, using only
information that would be known before the game starts. It compares three models
(logistic regression, random forest, and XGBoost) and checks how much better they
do than just always guessing the home team.


Contents
--------------

Three notebooks, which must be run in this order:

1. data_preparation.ipynb
   
2. modeling_and_tuning.ipynb

3. evaluation_and_analysis.ipynb

The final report is the PDF included in this repository.


GETTING THE DATA
----------------

You need two files in the same folder as the notebooks:

- Games.csv          (one row per game: date, teams, scores, winner)
- TeamStatistics.csv (two rows per game, one per team, with the box score)

These come from the public NBA dataset and are not included here because they are
too large for GitHub. Download them and put them in the same folder as the
notebooks before running anything.
https://www.kaggle.com/datasets/eoinamoore/historical-nba-data-and-player-box-scores


SETUP
-----
You need pandas, numpy, scikit-learn, xgboost, matplotlib, and jupyter.

HOW TO RUN
----------
Make sure Games.csv and TeamStatistics.csv are in the same folder as the notebooks
open each notebook and run it top to bottom, in this order:

1. data_preparation.ipynb
2. modeling_and_tuning.ipynb
3. evaluation_and_analysis.ipynb

The order matters, each notebook saves files that the next one reads, so running
them out of order will cause errors because the later notebooks will not find the
files they need.

As they run, the notebooks create some CSV files (modeling_table.csv,
test_predictions.csv, and a couple of others) and write the report figures into a
figs folder. These are generated outputs, not source files.


HOW IT WORKS
------------

The main thing this project is careful about is not cheating by accident. Box score
stats like points and rebounds are only known after a game is over. If you feed
them straight into the model, it already knows who won, and you get a fake accuracy
above 95% that falls apart on real predictions.

Two things prevent this:

1. Every rolling feature is shifted back by one game, so a game's features only use
   games that came before it.

2. The train/test split is by date. The oldest 80% of games are used for training
   and the most recent 20% for testing, instead of a random split that would let
   the model learn from future games.
