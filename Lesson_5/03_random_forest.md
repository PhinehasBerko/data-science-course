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
