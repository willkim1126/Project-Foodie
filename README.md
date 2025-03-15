# Project Foodie
Name: Junghwan Kim
PID: A16996831

## Introduction
Being a big food enthusiast, a large dataset of recipes sparked my curiosity in what how I can utilize the dataset and incorporate data science skills into creating something where I can deepen my food knowledge.

'You are what you eat'. By analyzing the dataset that contains over 83,000 data, I aim to understand 'food' in a higher dimension. 

### Research Question
In this project I aim to answer the question of 'How can we analyze recipes to....'

### About Data 
The dataset I'm looking is the recipes dataset from food.com, where it contains neccesary information about each recipes. The dataset contain 83782 rows (83782  recipes) and 14 columns (14 variable) for each row. 
In this project, columns 'minutes', 'tags', 'nutrients', 'n_steps', 'steps', 'ingredients' were the main columns that were used to analyze the dataset. 

| Column| Description| 
| -------- | -------- |
| 'minutes'   |  Minutes to prepare recipe |
| 'tags'  | Food.com tags for recipe  |
| 'nutrients' | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps' | Number of steps in recipe |
| 'steps'| Text for recipe steps, in order | 
| 'ingredients' | Ingredients used in the recipe |


## Data Cleaning and Exploratory Data Analysis 

### Data Cleaning
The provided dataset for the recipes were in two separate datasets: recipe datasets that contains recipes and ratings dataset that contains reviews and ratings submitted for the recipes in the other dataset. 

Therefore, merging the two dataset was neccesary. Left merge the recipes and interactions datasets together. Then, In the merged dataset, fill all ratings of 0 with np.nan. After filling the 0s with np.nan, average rating per recipe was found, in Series, for creating a new column called 'average_rating'.

It is also important to note that further data cleaning is done in the later part of the project as more analysis is done.

## Univariate Analysis
Starting the EDA, univariate analysis done first to understand the dataset better. 



