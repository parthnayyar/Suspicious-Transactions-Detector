# Suspicious-Transactions-Detector

## File Contents
File|Content
---|---
predictions.csv|This file contains the predictions of the current transactions.
---|---
resamplingRatioOptimizationAndTuning.ipynb|This file contains all the code where I preprocess historical and current transactions data, implement model selection, and optimize resampling ratios.
---|---
dataInspectionAndTesting.ipynb|This file contains all the code where I inspected the data, such as inspecting data types, null values, resampling techniques, and random testing to see if my code works.

## Data inspection findings
-	Some features are of “object” (string) data type:
  -	x5:  some kind of percentage, should be converted to float.
  -	x6:  day of the week (categorical), should be one-hot encoded.
  -	x20: months of the year (categorical), should be one-hot encoded.
  -	x49: true/false, should be converted to binary.
  -	x57: some kind of amount, should be converted to float.
-	Numerous null values throughout the dataset:
  -	For categorical features, use mode.
  -	For non-categorical features, use mean.
  -	Null values in X_test should be filled using means and modes of X_train
-	Data should be normalized.
-	Data is heavily imbalanced: 
  -	7561 unsuspicious transactions and only 439 suspicious transactions (95-5)
  -	Solutions include oversampling of minority class, undersampling of majority class, a combination of both, and consideration of metrics other than accuracy during model selection.
  -	Oversampling alone will probably result in overfitting due to the incredibly high 95-5 imbalance. 
  -	Undersampling alone will lead to loss of important information.
  -	I have hence used a combination of both undersampling and oversampling techniques to achieve reduced imbalance in the train dataset.
  -	When splitting data into train and test dataset, it is important to retain the imbalance.
  -	For model selection, I have taken into consideration the recall of the “suspicious” class. This is because recall is the most important metric in spam/suspicion detection.

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
