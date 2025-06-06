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
