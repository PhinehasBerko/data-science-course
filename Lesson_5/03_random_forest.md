# Ensemble Models: Random Forest

## we've learned how to retrieve and decompress data, and how to manage imbalanced data to build a decision-tree model.

In this lesson, we're going to expand our decision tree model into an entire forest (an example of something called an ensemble model);

### learn how to use

#### a grid search to tune hyperparameters;

#### create a function that loads data

#### and a pre-trained model,

#### and uses that model to generate a Series of predictions.

## Prepare Data

### Import

import gzip > for compressing and decompressing files
import json > for reading JavaScript Object Notation files
import pickle > for writing (saving) our model into a binary file

import matplotlib.pyplot as plt > for visualiazations
import pandas as pd > for data manupulation and analysis
import seaborn as sns > for data visualization

from imblearn.over_sampling import RandomOverSampler
from sklearn.ensamble import RandomForestClassifier
from sklearn.impute import SimpleImputer
from sklearn.metrics import ConfusionMetricDisplay
from sklearn.model_selection import train_test_split, GridSearchCV, cross_val_score
from sklearn.pipeline import make_pipeline

### Split > dataset into features vs target and train_set vs test_set

target = "bankrupt"
X = df.drop(columns= "bankrupt")
y = df[target]

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state = 1, test_size = 0.2)

### Resample > refers to the process of creating new samples or data points based on existing ones

we do this on the train data and not the test

over_sampler = RandomOverSampler(random_state = 42)
X_train_over, y_train_over = over_sampler.fit_resample(X_train, y_train)

## Build Model

### Baseline > Calculate the baseline accuracy score for your model

acc_baseline = y_train.value_counts(normalize = True).max()
print("Baseline Accuracy:", round(acc_balance,4))

### Iterate

Ensemble models work by building multiple models on random subsets of the same data, and then comparing their predictions to make a final prediction

we build a classifier by:

clf = make_pipeline(SimpleImputer(), RandomForestClassifier(random_state=42))

In order to get the best performance from our model, we need to tune its hyperparameter, one way is using a cross-validation
cv_acc_scores = cross_val_score(clf, X_train_over, y_train_over, cv = 5, n_jobs= -1)

## Evaluate

we evaluate to see how our model performs
To Calculate the training and test accuracy scores for model

acc_train = model.score(X_train, y_train)

acc_test = model.score(X_test, y_test)

print("Training Accuracy:", round(acc_train, 4))
print("Test Accuracy:", round(acc_test, 4))

The Confusion Matrix gives an indept view of how your best model performs on your test set.

ConfusionMatrixDisplay.from_estimator(model, X_test, y_test)

## Communicate

In communicating our results, we export our model in pickle file

def make_predictions(data_filepath, model_filepath): # Wrangle JSON file
X_test = wrangle(data_filepath) # Load model
with open(model_filepath, 'rb') as f:
model = pickle.load(f) # Generate predictions
y_test_pred = model.predict(X_test) # Put predictions into Series with name "bankrupt", and same index as X_test
y_test_pred = pd.Series(y_test_pred, index = X_test.index, name = "bankrupt")
return y_test_pred
