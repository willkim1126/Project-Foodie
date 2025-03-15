# Project Foodie
Name: Junghwan Kim\
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

### Univariate Analysis
Starting the EDA, univariate analysis done first to understand the dataset better. 

### Distribution of Average Rating 

<iframe
  src="assets/figure_18.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

### Average Rating vs. Cooking Time
<iframe
  src="assets/figure_17.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness

### NMAR Analysis 
From the dataset, we can see that the column with the most np.nan is the column 'average_rating'. When merging the two orignal datasets, it is very plausible that the not all recipes had any ratings from the ratings dataset, leading to missing values at 'average_rating' column. 

### Missingness Dependency
## Permutation Test Analysis for Recipe Description Missingness

This code implements a permutation test to investigate whether missing values in the 'description' column of a recipe dataset are related to other recipe attributes. The analysis begins by creating a binary indicator variable `description_missing` that flags recipes with missing descriptions (1) versus those with descriptions (0).

The permutation test function computes the observed mean difference between these two groups for each variable of interest, then creates a null distribution by randomly shuffling the data 1,000 times. For each permutation, it recalculates the difference in means and ultimately determines a p-value by measuring how often the permuted differences are as extreme as or more extreme than the observed difference.

The results reveal:

- **Minutes (cooking time)**: Observed difference = -43.8247, p-value = 0.5630
  - Recipes missing descriptions tend to have shorter cooking times on average, but this difference is not statistically significant.

- **Number of ingredients**: Observed difference = -1.4724, p-value = 0.0000
  - Recipes missing descriptions have significantly fewer ingredients (about 1.47 fewer on average).
  - The extremely low p-value indicates this relationship is highly unlikely to occur by chance.

- **Number of steps**: Observed difference = 0.9954, p-value = 0.1840
  - Recipes missing descriptions have slightly more steps on average, but this difference is not statistically significant.

These findings suggest that the missingness pattern in the description column is not completely random (MCAR) but may depend on recipe complexity as measured by ingredient count, indicating a potential Missing At Random (MAR) mechanism.


## Hypothesis Testing 
Moving deeper into understnding analyzing the recipe dataset, focus on nutrients of the recipes and figure out what constitute what type of food is healthy is important.

### Reseaarch Question
What types of recipes tend to be healthier?

### Key Definitions in Research Question 
Below is the defintions of the keywords 'types' and 'healthier' in the research question in terms of quantifiable, representable metrics in the dataset. \
\
'Types': Each recipe has a column called 'tags' which consists a list of tags realted to the recipe. The 'tags' will be the 'type' of recipes. Each recipes can have multiple types. \
\
'Healthier': Determine whether a recipe is healthy or not by looking at the column 'nutrition' which consist nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]. Specfically, consider protein (PDV) and sugar (PDV). If protein (PDV) is greater or equal to 20, sugar (PDV) is lower than 5, and carbohydrate (PDV) is less than 26. 

### Null Hypothesis
The types of recips does not affect its healthiness. Protein, sugar, and carbohydrate percent daily values are independent of recipe types.

### Alternative Hypothesis
The type of recipe affects its healtiness. Protein, sugar and carbohydrate percent daily values depend on recipe types.

### Data Prepocessing
In order to answer the research question, more data cleaning is required. the column 'nutrition' and 'tags' are both in string. However, they both have brackets [] inside, so changing to approriate type of list is necessary before the hypothesis test. Morever, we will dive the column 'nutrients' to different nutrient information.


### Undestaing the Data More
We need to further understand the dataset in order properly execture hypothesis testing. Through data wrangling, unique list of ingredients and tags in the dataset was obtained. This is will help hypothesis testing as filtering through 'healthy' recipe is needed. From this, there was 4913 healthy recipes in the dataset. 

### Determing the Type of Test
Since we are dealing with discrete, unpaired, categorical data type 'tags', we will use chi-square test. However, there's 549 unique tags in the dataset. Perforiming indiviual hypothesis test for each tage will not only take long and inefficient, it will increase the risk of Type 1 error. 








