# Flights Analysis

## Short description 

This is a Kaggle project: https://www.kaggle.com/datasets/ulrikthygepedersen/airlines-delay

In this project the chances of a flight being delayed or not are analysed. It is a binary classification problem. We train 3 models: XGBoostClassifier, CatBoostClassifier and RandomForestClassifier to predict whether a flights within the US is delayed or not. The dataset contains many categorical features, such as names of airports and names of airlines.

## Methodology

The original dataset contains 6 features: 'Time', 'Length', 'Airline', 'AirportFrom', 'AirportTo', 'DayOfWeek', which can influence the delay of a flight, with (1) - flight delayed and (0) - flight on time. We treat 'Length' as the only numerical variable. 'Time' means the time of a take-off of the plane and we treat it as categorical variable by extracting categories: night, morning, afternoon, evening. Other features are also rearranged in a way to extract some representative meaning of them. For example, 'Airline' is divided into 3 groups, which represent: standard, regional and cheap airlines. Data are standardized by self-defined scalers. Feature selection is performed by first removing overcorrelated columns, and then by using SelectKBest algorithm to leave 10 most impactful features. We see, however, that changing this number slightly does not influence the performance of the models significantly. We also do cleaning row-wise, which eliminates a lot of noise and stands for key improvement in the final results. With back and forth analysis, we observe that only the combination of EditedNearestNeighbours and InstanceHarnessThreshold algorithms can lead to impressive improvement of the metrics. When training the models, we first start with a basic model version with intuitive set of hyperparameters. Then we apply GridSearchCV to optimize the hyperparameters. Finally, we calculate all matrics for the best version of each estimator. Only hyperparameter space should be adjusted by hand, the rest of the code is automated.

## Results and models performance

Comparing the performance of the three models, we see that the best metric are obtained by CatBoostClassifier. For example: accuracy - 99.07% (test), 99.33% (train) and logloss - 0.066, for other metrics see the report files. Similar, very good performance is visualised by the confusion matrix, precision-recall and ROC curves, and also the distribution of probability in the form of histogram. We point out that optimization of hyperparameters increased the accuracy by about 7%. The performance of XGBoostClassifier is not much worse. For comparison: accuracy - 97.07% (test), 97.46% (train) and logloss - 0.082, but the time needed for training is a few times shorter. Given that these two estimators are very efficient in training and predictions, RandomForestClassifier should be considered rather as a weak model for this analysis with the accuracy being only 74.09% for test data.

As already mentioned, we also tested XGBoostClassifier on data (called experimental), whose rows were not cleaned from noise, and compared to the same model with cleaned data. It was done with initial set-up of hyperparameters of each model for convenience. While having two classes in the target, one knows that double-peaked probability distribution should be observed. The model trained on the uncleaned data generated one-peak probability distribution centered around *p*=0.5 and led to large overfitting of the test data (see the plots and metrics in *models_results*). This was a clear sign to optimize the instances of the dataset carefully.

## File content

The project consists of the following parts. The exploratory analysis and data cleaning is included in *preprocessing_flights.ipynb*. The source files both before the preprocessing and generated after the preprocessing are included in the folder *src*. The main part for models training is given in *model_definition.py*, where the preprocessed data are split and shuffled, all functions for model's training and metrics visualisation are defined, and the algorithms are initialized. The files: *xgb_classifier.py*, *cat_boost_classifier.py* and *random_forest_classifier.py* contain model training set-up and hyperparameters tunning. All three models are saved in the folder *models* and all results on the performance of each model are kept in the folder *models_results*. The file *xgb_classifier_exp.py* icludes an experimental XGBoost model performing on data without instances cleaning to show how much uncleaned, noisy data affect the model performance.

