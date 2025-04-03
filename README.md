Junghwan Kim\
junghwk11@gmail.com

## Introduction
Being a big food enthusiast, a large dataset of recipes sparked my curiosity in what how I can utilize the dataset and incorporate data science skills into creating something where I can deepen my food knowledge.

'You are what you eat'. By analyzing the dataset that contains over 83,000 data, I aim to understand 'food' in a higher dimension. 

### Research Question
In this project I aim to answer the question of 'How do different recipe characteristics (e.g., ingredients, cooking time, and tags) impact their healthiness?'

### About Data 
The dataset that was used in this project was originally scraped from [food.com](https://www.food.com/) and was scraped by the awsome authors of the paper called [Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf). 

The recipe instruction dataset contains 83,782 recipes, each with 14 attributes detailing cooking time, ingredients, and nutritional information. The recipes interaction dataset contains over 200,000 reviews of recipes in the recipes dataset. 
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

Therefore, merging the two dataset was necessary. A left merge was used to ensure that all recipes remain in the dataset, even if they have no reviews. Then, In the merged dataset, fill all ratings of 0 with np.nan. After filling the 0s with np.nan, average rating per recipe was found, in Series, for creating a new column called 'average_rating'.

It is also important to note that further data cleaning is done in the later part of the project as more analysis is done.

### Univariate Analysis
Starting the EDA, univariate analysis done first to understand the dataset better. 

### Distribution of Average Rating 

<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

### Average Rating vs. Cooking Time
<iframe
  src="assets/steps_vs_avg_rating.html"
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

These findings suggest that missing descriptions are associated with recipe complexity (fewer ingredients). This indicates a Missing At Random (MAR) mechanism, meaning that certain types of recipes are more likely to have missing descriptions, potentially affecting our analysis of recipe characteristics.


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
In order to answer the research question, more data cleaning is required. the column 'nutrition' and 'tags' are both in string. However, they both have brackets [] inside, so changing to appropriate type of list is necessary before the s. Morever, we will dive the column 'nutrients' to different nutrient information.


### Understanding the Data More
We need to further understand the dataset in order properly execture hypothesis testing. Through data wrangling, unique list of ingredients and tags in the dataset was obtained. This is will help hypothesis testing as filtering through 'healthy' recipe is needed. From this, there was 4913 healthy recipes in the dataset. 

### Determing the Type of Test
Since we are dealing with discrete, unpaired, categorical data type 'tags', we will use chi-square test. However, there's 549 unique tags in the dataset. Perforiming indiviual hypothesis tests for each tage will not only take long and inefficient, it will increase the risk of Type 1 error. 

### Recipe Tag Analysis: Preparing for Chi-Square Testing

The chi-square test preparation implemented here transforms recipe tag data into a format suitable for statistical analysis of health associations. Let's break down the process:

### Step 1: Converting Tags to Binary Indicators
First, the implementation creates binary indicators for each unique tag in the dataset. Rather than working with complex tag lists, each recipe now has a series of 1s and 0s indicating whether specific tags are present or absent.

### Step 2: Expanding the Dataset
These binary indicators are then added to the original recipe dataset, preserving all initial information while incorporating the structured tag data.

### Step 3: Building Contingency Tables
For each tag, a 2×2 contingency table is constructed to examine its relationship with recipe healthiness:

| Tag Presence | Healthy Recipes | Unhealthy Recipes |
|--------------|-----------------|-------------------|
| Tag Present  | A               | B                 |
| Tag Absent   | C               | D                 |

Where:
- **A**: Number of healthy recipes with the tag
- **B**: Number of unhealthy recipes with the tag
- **C**: Number of healthy recipes without the tag
- **D**: Number of unhealthy recipes without the tag

The implementation includes safeguards to ensure all counts remain non-negative, which is essential for valid chi-square testing.

These contingency tables provide the foundation for subsequent chi-square tests that will reveal which tags have statistically significant associations with recipe healthiness.

### Chi-Square Analysis: Tag Association with Recipe Healthiness

The chi-square analysis implemented here examines the relationship between recipe tags and healthiness classification through a systematic statistical approach:

### Methodology
- **Contingency Table Analysis**: For each tag, a 2×2 contingency table was constructed showing the distribution of healthy/unhealthy recipes with/without the tag
- **Chi-Square Testing**: The `chi2_contingency` function calculated p-values indicating the probability that observed tag-health associations occurred by chance
- **Multiple Testing Correction**: Bonferroni correction was applied to adjust p-values, controlling for the increased risk of false positives when conducting multiple tests
- **Significance Filtering**: Only tags with adjusted p-values below 0.05 were retained as statistically significant

### Key Findings
- **Widespread Significance**: The analysis revealed 256 tags with statistically significant associations to recipe healthiness
- **Extremely Strong Associations**: Many tags showed remarkably low p-values (as low as 5.63e-176 for "5-ingredients-or-less")
- **Diverse Predictors**: Significant tags spanned various categories:
  - Preparation complexity ("5-ingredients-or-less")
  - Time investment ("1-day-or-more", "weeknight")
  - Specific ingredients ("white-rice", "whole-duck")

### Aftermath
These findings demonstrate that recipe tags serve as powerful indicators of healthiness. The statistical strength of these associations suggests that certain ingredients, preparation methods, and cooking styles are systematically related to health classifications in recipes. This information could be valuable for recipe recommendation systems, nutritional analysis, or developing healthier cooking alternatives.
Moreover, after filtering through recipes that only include the healthy tags, 102 recipes were found to be healthy.

### Conclusion
Out of 549 hypothesis tests, we were able to reject the null hypothesis for 256 tags which are stored in the variable 'healthy tags'.\
\
In other words out of 549 unique recipe tags, the hypothesis tests identified 256 tags that are statistically significant in their association with being healthy. These 256 “healthy tags” were then used to filter recipes, resulting in 102 recipes that exclusively consist of these healthy tags. This represents a small subset of over 2 million recipes analyzed.


## Framing a Prediction Problem 
To incorporate machine learning, the next steps are related to building a predicitve model based on the recipe dataset.

### Problem Identification 
The goal of this prediciton problem is to predict number of minutes it takes to make a recipe based on the given ingredients and tags. Based on the ingredients and tags give, the model aims to predict the complexity of the recipe. In order to quantify the defintion of 'complexity' here, column 'minutes' was used as it is intuitive to say the longer it takes, more complex the recipe is. Looking at the number of ingredients, we can say that more ingredients mean its more complex to cook, affecting the response variable, minutes. Similarly, specfic tags can be related to recipe being more or less complex. 

### Evaluation Metric 
For model evaluation, Mean Square Error and R^2^ to evalute the model. Since the regression model predicts a continuous variable of cooking time, MSE would be a useful metric as it works well with for optimization in machine learning models. It will also penalize larger errors more heavily. R^2^ would also be a useful metric as it explains varaince in the data and also scale-independent. 

## Base Model 
Again, the dataset needs further cleaning and preprocessing in order for model to achieve its intended purpose. 
First, only relvant columns in recipes were used to lower the memory used in the cpu (since the model will blow up my laptop if I run the whole, raw dataset). The next few sections will provide more details in data preprocessing.

### One-hot Encoding for 'Ingredients' and 'Tags' Columns
Here, we use MultiLabelBinarizer to perform one-hot encoding on the ‘ingredients’ and ‘tags’ columns. The `sparse_output=True` parameter creates a sparse matrix, which is memory-efficient for high-dimensional data with many zero values.

### Combining the Encoded Columns to the Original Dataframe
After, the encoded ingredients and tags are combined into a single sparse DataFrame. Using sparse representations significantly reduces memory usage for high-dimensional, sparse data.

### Pipeline
The main pipe line of the model is as followed:
- SelectKBest chooses the top 1000 features based on mutual information with the target variable. This reduces dimensionality while retaining the most relevant features.
- Principal Component Analysis (PCA) further reduces the dimensionality to 100 components. PCA finds the directions of maximum variance in the data, allowing us to represent the data with fewer dimensions while retaining most of the information.
- StandardScaler standardizes the features by removing the mean and scaling to unit variance. This is important for many machine learning algorithms, including Random Forest, to ensure all features are on a similar scale.
- The Random Forest Regressor is used for prediction. It’s an ensemble of decision trees, which is well-suited for handling non-linear relationships and interactions between features.

### Fitting
All the effort to reduce dimensionality of the data was worth it as the training was finished in around 55 minutes. The onehot encoding of over 10,000 uniqe ingredients was why it was necessary to reduce dimensionality. 

### Performance
- **Mean Squared Error:** 67260671.69
- **R² Score:** -0.00

Based on the evaluation metric, we can we see that our model performing horribly. Very horribly. The MSE suggest that the model has extremely large prediction errors. The RMSE is around 8201 minutes based on MSE,  which is an enormous error for predicting cooking time. The R^2^ score of 0.00 indicates that the model performs no better than simply predicting the mean value for all samples. Moreover, it's negative, telling us that it's worse than just predicitng mean. Ultimately, this suggests the model has failed to capture any meaningful relationship between features and target.

## Improved Model 

### Exploring the 'minutes' column
One of the causes of the poor performance of the model might be due to extreme outliers in the 'minutes' column. Further analysis on the data is much needed.

<iframe
  src="assets/minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Look at the plot above and its x axis. By the distribution of the plot, you can clearly tell there are some insane outliers. 

In order to find outliers, modified z-scores test was performed, finding 7496 outliers, based on their z-score, that was higher than 3.5.


### Running the Model Again
After removing the outliers from the dataset that is fed through the model, the model ran quicker as well, in the range of 37 minutes. 

### Revised Performance
- **Mean Squared Error:** 125.55
- **R² Score:** 0.84

Looking at the MSE, we can see that it is significantly lower than the orginial metric. The square root (RMSE) would be about 11.2 minutes, which is a reasonable error margin for predicting cooking times. Moreover, the $R^2$ is much better than the -0.00 socre from the preivous result. $R^2$ value of  0.84 means the model explains approximately 84% of the variance in cooking times.


## A new approach to the Final Model 

### Two New Features 
The new features or variables in the dataset I thought it would better the base model was: calories and n_steps. If the meal had more calories, it could mean that it will have more ingredients or have bigger portion size, leading to higher complexity of recipe, affecting the minutes. In contrast, less calories might mean less ingredients and less portion size, leading to lower complexity of recipe, affecting the minutes. Number of steps is very intuitive since it will directly influence the complexity of the recipe. 
 

## Fairness Analysis
Again, due to time constraint I was not able to run the model again and actually run the permuatation test. My approach was to divide into two groups, based on the number of steps, group X being recipes with less than 10 steps, and group y being recipes with more than 10 steps.
