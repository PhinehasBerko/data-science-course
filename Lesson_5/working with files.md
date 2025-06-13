In this project, we'll be looking at tracking corporate bankruptcies in Poland.
To do that, we'll need to get data that's been stored in a JSON file, explore it,
and turn it into a DataFrame that we'll use to train our model.

JSON, short for JavaScript Object Notation, is a lightweight data-interchange
format used for storing and transmitting data.

It's based on a subset of JavaScript object syntax, making it easy for both humans and machines to read and write.
JSON is often used in web applications for sending data between servers and clients

gzip is for compressing and depressing files

## Prepare Data

### Open

pwd >> present working directory

cd >> current directory

datasets can be large or small, messy or clean, and complex or easy to understand. Regardless of how the data looks, though, it needs to be saved in a file somewhere, and when that file gets too big, we need to compress it.

Decompress Files: Command Line

Gzip is a popular lossless data compression utility and format,
commonly used to reduce the size of files, primarily for storage
and transmission over networks like the internet

gzip -dfk file_name (where -d >> decompress -f >> force and replace existing file with the same name -k keep the original compressed file)

Decompress File : Jupyter

### To run command line or bash code in Jupyter notebooks >> %%bash

This makes the cell behave as a terminal to accept cmd commands

### To inspect JSON: command Line we use head file_name

For example: head poland-bankruptcy-data-2009.json

#### NB:

It's important to note that JSON is not exactly the same as a dictionary,
but a lot of the same concepts apply

### Reading JSON file into a dictionary using Python

import json

with open("path/to/file/data_source", "r") as read_file:
data = json.load(read_file)

### Convert dictionary to DataFrame and set the index column

df = pd.DataFrame().from_dict(poland_data_gz["data"]).set_index("company_id")
print(df.shape)
df.head()

### Create a wrangle function that takes the name of a compressed file as input and returns a tidy DataFrame

def wrangle(filename): # Open compressed file, load into dict
with gzip.open(filename) as read_file:
dataset = json.load(read_file)

    #Transform data into a data frame
    dataFrm = pd.dataFrame().from_dict(dataset["data"]).set_index("company_id")

    return dataFrm

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
