## Lesson 5.2 GOALS

### 1. Identify imbalanced data and address it with resampling.

### 2. Build a model to predict bankruptcy.

### 3. Evaluate model predictions using a confusion matrix.

### 4. save our model

In this lesson, we're going to explore some of the features of the dataset, use visualizations to help us understand those features, and develop a model that solves the problem of imbalanced data by under- and over-sampling

### Highligths or Workflow

## 1. Prepare Data

### 1. Import

### 2. Explore

### 3. Split

### 4. Resample: Under- and over-sampling

## 2. Build Model

### 1. Baseline

### 2. Iterate

### 3. Evaluate : Confusion Matrix

## 3. Communicate Results

### 1. Feature Importances

### 2. Save model as file

#### some libraries to import

import pickle
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import wqet_grader
from imblearn.over_sampling import RandomOverSampler
from imblearn.under_sampling import RandomUnderSampler
from sklearn.impute import SimpleImputer
from sklearn.metrics import ConfusionMatrixDisplay
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.tree import DecisionTreeClassifier

## 1. Prepare Data

### 1. Import and Load data

def wrangle(filename): # Open compressed file compressed file manager and load it in a dictionary
with gzip.open("filename", "r") as read_file:
dataset = json.load(read_file)

    # Transform the dictionary to a dataFrame and using company_id as index column
    dataFram = pd.dataFrame().from_dict(dataset["data"]).set_index("company_id")

    return dataFram

## Explore

### To inspect the dataset we use the .info() method to:

### 1. check which features there are in the dataset and their data types.

### 2. What could / is the target variable.

### 3. Are there missing values in the dataset.

As always, it's a good idea to do some visualizations to see if there
are any interesting trends or ideas we should keep in mind while we work

To begin with, bar chart (plots) helps us to know the various classes
(categories) within a particular variable (column).

To explore the distributions of features, boxplot is a help means

When a feature in a dataset is skewed, it is important to keep in mind
the type of model to use

Another important consideration for model selection is whether there are any issues with multicollinearity in our model

Multicollinearity in statistics refers to a situation where two or more predictor variables in a regression model are highly correlated with each

### It is important to know that "good" accuracy scores don't tell us much about the model's performance when dealing with imbalanced data

let's see how its predictions differ for the two classes in the dataset.

### Confusion Matrix

A confusion matrix is a table that summarizes the performance of a classification model by comparing its predicted labels to the true labels. It visualizes the counts of true positives (TP), true negatives (TN), false positives (FP), and false negatives (FN). This table is crucial for understanding where a model gets confused and making informed decisions about model optimization
