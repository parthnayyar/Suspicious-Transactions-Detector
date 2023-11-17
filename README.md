# Suspicious-Transactions-Detector

## File Contents
File|Content
----|-------
predictions.csv|This file contains the predictions of the current transactions.
resamplingRatioOptimizationAndTuning.ipynb|This file contains all the code where I preprocess historical and current transactions data, implement model selection, and optimize resampling ratios.
dataInspectionAndTesting.ipynb|This file contains all the code where I inspected the data, such as inspecting data types, null values, resampling techniques, and random testing to see if my code works.

## Data inspection findings
- a
  - a

## Methodology and process
-	Split historical data into X_train, y_train, X_test, y_test with stratified split to retain imbalance.
-	Preprocess X_train (deal with string features, one-hot encoding, null values, normalization)
-	Preprocess X_test (deal with string features, one-hot encoding, null values using X_train means and modes, normalization)
-	Optionally undersample and oversample X_train and y_train based on ratios input into function.
-	Model Selection:
  -	Tuning is done for various combinations of undersampling and oversampling ratios (including no resampling at all)
  - SVM, Random Forest Classifier, Logistic Regression, and K-Nearest Neighbors (with k=1,…,5) models are fitted for all resampling ratios and the recall of the models is compared.
  -	The model and resampling ratios with an accuracy of at least 90 and the highest recall are stored.
-	Data from current_transactions.csv is preprocessed (deal with string features, one-hot encoding, null values using historical data means and modes, normalization)
-	Any missing features in best_X_train and current datasets are dealt with (there might be missing one-hot encoded features) and best training data is refitted to the best model.
-	Predictions for current transactions are made and written to predictions.csv.
