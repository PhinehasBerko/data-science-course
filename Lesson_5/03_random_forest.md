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
