# A/B TESTING

This project is an example of A/B testing, also known as a randomized controlled experiment. In the private sector, A/B tests are used improve things like email marketing and product pricing. In politics, they can be used to test campaign messages. Scientists of all kinds use them in their research.

Once you complete this project, you'll be able to:

Build a choropleth map to show the distribution of ADSL students around the world.
Create a custom Python class to implement ETL processes.
Design an experiment and analyze the results using a chi-square test.
Build an interactive web application that follows a three-tiered design pattern.

These lessons are designed to build your understanding step-by-step:

071: Explore applicant demographics and create geographical visualizations
072: Build ETL classes to organize data processing pipelines
073: Apply chi-square testing to analyze relationships in categorical data
074: Create interactive dashboards to present your findings

The saddest aspect of life right now is that science gathers knowledge faster than society gathers wisdom.
— Isaac Asimov

## Meet the DS Lab Applicants

#### Brief Bio

When you decided to start down the path to becoming a data scientist at MyQuest, the first thing you did was to register an account with us. Then you took our admissions exam test, and began your data science journey! But not everyone who creates an account takes the admissions exam. Is there a way to improve that completion rate?

In this project, you'll help run an experiment to see if sending a reminder email to applicants can increase the likelihood that they'll complete the admissions exam. This type of experiment is called a hypothesis test or an A/B test.

In this lesson, we'll try to get a better sense of what kind of people sign up for MyQuest Lab — where they're from, how old are they, what have they previously studied, and more.

some libraries to import

- from pprint import PrettyPrinter

- import pandas as pd
- import plotly.express as px
- from country_converter import CountryConverter

* from pymongo import MongoClient

## First, Instantiate a PrettyPrinter, and assign it to the vairable pp

pp = PrettyPrinter(indent = 2)

## 2nd, connect to the MongoDB server.

Create a client that connects to the database running at host on port 27017.

client = MongoClient(host = host, port = 27017)

## 3rd, Print a list of the databases available on client

pp.pprint(list(client.list_databases()))

## 4th, Assign the "ds-applicants" collection in the "wqu-abtest" database to the variable name ds_app

db = client["wqu-abtest"]
ds_app = db["ds-applicants"]

## Explore

#### Use the count_documents method to see how many documents are in the ds_app collection.

ds_app.count_documents({})

so, this gives the number of individual records in the collection

#### Use the find_one method to retrieve one document from the ds_app collection and assign it to the variable name result

result = ds_app.find_one({})
pp.pprint(result)

- Let's start the analysis \*

### Nationality

One of the possibilities in each record is the country of origin.
but we can figure out just _how_ diverse it is
by seeing where applicants are coming from.

1st, we'll perform an aggregation to count countries

##### Use the aggregate method to calculate how many applicants there are from each country.

result = ds_app.aggregate(
[
{
"$group":{
"_id":"$countryISO2",
"count": {"$count":{}}
}
}
]
)

Task 7.1.8: Put your results from the previous task into a DataFrame named df_nationality. Your DataFrame should have two columns: "country_iso2" and "count". It should be sorted from the smallest to the largest value of "count"

df_nationality = (
pd.DataFrame(result).rename({"\_id":"country_iso2"},axis= "columns").sort_values("count")
)

#### Instantiate a CountryConverter object named cc, and then use it to add a "country_name" column to the DataFrame df_nationality

cc = CountryConverter()
df_nationality["country_name"] = cc.convert(
df_nationality["country_iso2"], to = "name_short"
)

Task 7.1.10: Create a horizontal bar chart of the 10 countries with the largest representation in df_nationality. Be sure to label your x-axis "Frequency [count]", your y-axis "Country", and use the title "DS Applicants by Country"

fig = px.bar(
data_frame = df_nationality.tail(10),
x = "count",
y = "country_name",
orientation = 'h',
title = "DS Applicants by Country"

)

fig.update(xaxis_title = "Frequency [count]", yaxis_title = "Country")
fig.show()

Task 7.1.13: Add a column named "country_iso3" to df_nationality. It should contain the 3-letter ISO abbreviation for each country in "country_iso2".

df_nationality["country_iso3] = cc.convert(df_nationality["country_iso2], to = "ISO3")

Task 7.1.14: Create a function build_nat_choropleth that returns plotly choropleth map showing the "count" of DS applicants in each country in the globe. Be sure to set your projection to "natural earth", and color_continuous_scale to px.colors.sequential.Oranges.

def build_nat_choropleth():
fig = px.choropleth(
data_frame = df_nationality,
locations = "country_iso3",
color = "count_pct",
projection = "natural earth",
color_continuous_scale = px.colors.sequential.Oranges,
title = "DS Applicants: Nationality"
)
return fig
