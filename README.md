# Happiness-Score-Regression

As the world slowly emerged from the post-covid area, people had time to re-access what was truly important in their lives. 

The goal of the project was to apply a broad variety of machine learning models to explore using quantitative, readily available statistical data instead of predominantly survey data to predict the World Happiness Score.

The project aims at private individuals and governments but could be tweaked to cater for companies by using business relevant quantitative features instead.

## Table of Contents
<div class="alert alert-block alert-info" style="margin-top: 20px">

1.  [Notebook Contents](#0)<br>
2.  [Project Goal](#2) <br>
3.  [Data Sources](#3) <br>   
4.  [Cleaning & Processing](#4)<br>
5.  [Features](#5)<br>
6.  [Exploratory Data Analysis](#6)<br>
7.  [Modelling](#7)<br>
8.  [Feature Importance](#8)<br>
9.  [The Happiest and Unhappiest Countries](#9)<br>
10. [Limitations](#10)<br>
11. [Key Learnings](#11)<br>
12. [Next Steps](#12)<br>
    </div>
    <hr>

## Notebook Contents <a id="0"></a>
Content | Notebook Name
--- | ---
Executive Summary and Data Acquisition | Data Acquisition
Exploratory Data Analysis | EDA
Modelling | Modelling

## Project Goal <a id="2"></a>
The goal of the project is to create a regression model which can use country level data, such as age dependency ratio, forest cover and urban population data, to outperform the baseline accuracy of using a multiple regression model to predict the Happiness Score with the originally suggested features.

The World Happiness Score is in a range between 0 to 10 with a dystopian country scoring equal or below 2.43.

## Data Sources <a id="3"></a>
Gallup is a global analytics and advice firm, with 90% of Fortune 500 companies using their data and 80 years plus data collection experience. Further, 99% of the world’s population is represented in Gallup’s World Poll and 160 countries with data in the Gallup’s Analytics database. 

The nine additional data sets were sourced from the World Health Organisation (WHO) and the United Nations (UN). On average, of about 2900 observations and an average of 4 years each, for data back over half a century in time. Features ranged from alcohol related to urbanisation and education to women in management and more.

## Cleaning & Processing <a id="4"></a>
The original and the additionally required data was well maintained, the data cleaning requirements were minimal and processing revolved more around:

- Removing countries and years not matching the original data
- Renaming columns in order to join files on year and country name 
- Re-label predictor names for user-friendliness
- Exploring percentages of missing data and outliers
- Identifying relevant and unusable predictors
- Investigate imputation methods 

## Features <a id="5"></a>
Index | Full Name | Column Name | Source
--- | --- | --- | --- 
1 | Country Name | country | Original and Additional Data
2 | Year | year | Original and Additional Data
3 | GDP per capita | log_gdp | Original Happiness Survey Data
4 | Social support | social_s | Original Happiness Survey Data
5 | Life expectancy | life_ex | Original Happiness Survey Data
6 | Freedom to make life choices | life_choices | Original Happiness Survey Data
7 | Generosity | generosity | Original Happiness Survey Data
8 | Perceptions of corruption | corruption | Original Happiness Survey Data
9 | Positive affect | positive_a | Original Happiness Survey Data
10 | Negative affect | negative_a | Original Happiness Survey Data
11 | Alcohol consumption per person | tot_alc_cons | Additionally acquired Data
12 | Population (historical estimates) | population | Additionally acquired Data
13 | Spirits consumption per person | spirit_c | Additionally acquired Data
14 | Wine consumption per person | wine_c | Additionally acquired Data
15 | Beer consumption per person | beer_c | Additionally acquired Data
16 | Forest cover | forest_c | Additionally acquired Data
17 | Urban Population | u_pop | Additionally acquired Data
18 | Age dependency ratio | age_dr | Additionally acquired Data
19 | Women in management positions | women_mgmt | Additionally acquired Data
20 | Share no formal education | r_noedu | Additionally acquired Data
21 | Happiness Score of countries | happiness_s | Original Happiness Survey Data

## Exploratory Data Analysis <a id="6"></a>
There were 3 features missing between 61% and 90% of their data; those were the ratio of people without formal education (90%), total alcohol consumption (77%) and women in management with (61%). Our subsequent investigation resulted in having to drop people without formal education and replace total alcohol consumption with subcategories. I couldn’t find any consistent pattern, data missing for developing and developed nations, and therefore ended up dropping this predictor. 

![Vis_Missing_Data_per_column](https://user-images.githubusercontent.com/100363396/158251644-eb3269c3-437a-4bda-81e7-fff1b9fadc06.png)

Of the remaining predictors, only corruption (6%) missed our threshold of 5% missing data. According to my research [(Schafer, 1999)](https://www.sciencedirect.com/science/article/pii/S0895435618308710#bib16), imputing variables below that threshold sould not produce anything more than a negligible bias to our features.

![Vis_Imputation_Impact_Corruption](https://user-images.githubusercontent.com/100363396/158251858-337de4e6-4964-44f6-89b6-1146f9203c3d.png)

The below table summarises the variance in the original data when compared to using various imputation methods. Median imputation is the best imputation solution.

![imputation_data_frame](https://user-images.githubusercontent.com/100363396/158251942-ef77dada-db8d-4903-a004-5846ebd2059b.png)

There weren’t a substantial amount of Outliers in our data, hence the data being from a reliable source. I assume there is no error of measurement not incorrectly entered data and consequently decided to standard scale our data to minimise the impact of the outliers on our models. 

![Vis_Boxplot_5_Outliers](https://user-images.githubusercontent.com/100363396/158252027-842a24d7-40d9-4baf-860a-4f85a7ddd5dc.png)

## Modelling <a id="7"></a>
Instead of guessing which model might perform best for our data, I ran multiple regression and tree based models on our data. The best performing model was the Random Forest, with its advantage of best dealing with de-correlating the predictors. I optimised the best performing model using K-Fold and Grid Search and finally applied that model to both our original and our newly acquired data. 

Although our training scores sometimes suggest over-fitting, the cross validation scores (5 each), and their average being close to our Test R2 score, dismiss that problem. 

### Initial Modelling

![performance_df](https://user-images.githubusercontent.com/100363396/158252287-d0974e52-eeba-4499-b1a1-dc9f9a71a2b9.png)

### Optimising

![kf_gs_df](https://user-images.githubusercontent.com/100363396/158252311-41011c5f-5b9d-47da-9456-ef83793be0d7.png)

### Original Data vs Box Standard

![lm_data](https://user-images.githubusercontent.com/100363396/158252340-bdcd6f7d-e4bc-4b28-b901-468ca849922a.png)

## Feature Importance <a id="8"></a>

![Random Forest Feature Importance Original Features](https://user-images.githubusercontent.com/100363396/158252439-d6b88b82-3156-495b-9c90-33a73a1e9b20.png)
There were also some surprises in terms of feature relevances in both datasets, the original and the newly acquired features, which are worth investigating further.

## The Happiest and Unhappiest Countries <a id="9"></a>

![68_Happiest_Observations](https://user-images.githubusercontent.com/100363396/158252541-a2f1d492-50c6-4e1d-ba41-11dc20e655dc.png)

![68_Unhappiest_Observations](https://user-images.githubusercontent.com/100363396/158252561-93b105d4-1914-4c91-960b-01ae13a3ebb7.png)

One yearly statistic encompasses 136 countries. That’s why I decided to analyse our data based on the top and bottom 68 observations across all of our dataset. 

## Limitations <a id="10"></a>

### Predictors
Ideally, I would have liked to have more de-correlated predictors in our newly gained data. This forced us to keep as many as possible, although there were several predictors with moderate collinearities and even a few with strong collinearities. 

### Imputation
Our lack of data, only 15 years, forced us to impute corruption data with the mean. Had I had more data available, it would have been better to drop that 6% data instead of imputing it. 

## Key Learnings <a id="11"></a>
I gained hands-on experience with some additional libraries, as well as the XGBoost model and the importance and selection of imputation methods.

Box standard measurements are more likely to produce less biassed results because they are less subjective, ‌increasing our model’s score and RMSE at the same time. 

It is interesting how our own perception and expectation can fool us, about which features contribute the most and the least to the precision of our prediction. Further, I noticed that the top 3 features explain roughly ¾ of the variance in the Happiness Score. More is not necessary, always better, but always worth exploring. 

## Next Steps <a id="12"></a>
Research some more variables in the attempt to remove some collinearity within the predictors, drop more features and engineer some additional features.  

Investigate p-values of our features to remove statistically insignificant features from our models. 

Build a categorical model, because it would be easier to interpret instead of a numerical score having categories (Very Happy, Averagely Happy, Unhappy) for a target value which is fairly normally distributed. 
