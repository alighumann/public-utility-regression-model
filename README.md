# public-utility-regression-model
H&S Revenue Forecasting with Seasonal Regression

Project Overview

This project develops and compares two multiple linear regression models for forecasting monthly revenue at H&S (Hotazel Steam).

The analysis examines whether the relationship between production and revenue changes across seasons. Seasonal dummy variables and interaction terms are used to allow both the regression intercept and the effect of production on revenue to differ during winter and summer.

The models are trained using monthly observations from 2011 to 2013 and evaluated using 2014 holdout data.

Business Objective

The objective is to identify a forecasting model that:

Captures the seasonal nature of H&S's operations

Explains how production translates into revenue during different seasons

Produces accurate out-of-sample revenue forecasts

Can support management's planning and forecasting process

Dataset

The analysis uses AICPA_regressionAnalysisData.csv.

The dataset contains monthly observations with the following variables:

Variable

Description

type

Identifies each row as training or testing data

date

Month-end date

revenue

Monthly revenue

production

Monthly production volume

coolDD

Cooling degree days

heatDD

Heating degree days

The data is divided into:

Training set: 2011-2013

Testing set: 2014

The 2014 observations are not used to estimate the models. They are reserved to evaluate forecast accuracy on unseen data.

Methodology

1. Data preparation

The notebook:

Imports the required Python libraries

Loads the CSV dataset

Converts the date column to a datetime format

Separates the training and testing observations

2. Seasonal dummy variables

Two binary variables are created:

winter_DV = 1 for December, January, and February

summer_DV = 1 for June, July, and August

Spring and fall serve as the baseline period when both dummy variables equal zero.

3. Interaction terms

The following interaction terms are created:

winter_interaction = production * winter_DV
summer_interaction = production * summer_DV

A seasonal dummy changes the intercept of the regression equation. An interaction term allows the slope on production to change during that season.

Models

Model 1: Winter-adjusted model

Model 1 includes:

Production

Winter dummy variable

Winter-production interaction

The estimated equation is:

Revenue = 5,629,257.08
          + 13.51(Production)
          - 201,742.73(Winter)
          + 14.16(Production x Winter)

This produces two seasonal relationships:

Non-winter:
Revenue = 5,629,257.08 + 13.51(Production)

Winter:
Revenue = 5,427,514.35 + 27.67(Production)

Model 2: Winter- and summer-adjusted model

Model 2 includes:

Production

Winter dummy variable

Summer dummy variable

Winter-production interaction

Summer-production interaction

The estimated equation is:

Revenue = 4,060,734.59
          + 18.87(Production)
          + 1,366,779.75(Winter)
          + 1,392,019.04(Summer)
          + 8.80(Production x Winter)
          - 6.28(Production x Summer)

This produces three seasonal relationships:

Spring/Fall:
Revenue = 4,060,734.59 + 18.87(Production)

Winter:
Revenue = 5,427,514.34 + 27.67(Production)

Summer:
Revenue = 5,452,753.63 + 12.59(Production)

The results indicate that an additional unit of production is associated with the most revenue during winter and the least revenue during summer.

Model Evaluation

The models are compared using:

Adjusted R-squared: Measures how much variation in training-period revenue is explained while accounting for the number of predictors

Mean Absolute Percentage Error (MAPE): Measures average forecast error on the 2014 testing data

Model

Adjusted R-squared

Test MAPE

Model 1

0.752

15.90%

Model 2

0.757

14.88%

Recommendation

Model 2 is the recommended forecasting model.

It performs better on both evaluation measures:

Higher adjusted R-squared

Lower out-of-sample MAPE

Adding summer variables improves the model because the effect of production on revenue is not constant throughout the year. Model 2 captures separate winter, summer, and spring/fall revenue relationships.

Although the improvement is moderate, Model 2 provides a more complete representation of H&S's seasonal business activity and produces more accurate forecasts for the 2014 testing period.

Visualizations

The notebook includes:

A comparison of actual 2014 revenue against forecasts from both models

Seasonal regression lines for the recommended model

Separate winter, summer, and spring/fall production-revenue relationships

Technologies Used

Python

Google Colab

pandas

NumPy

statsmodels

Matplotlib

Repository Structure

.
├── Quiz_8.ipynb
├── AICPA_regressionAnalysisData.csv
└── README.md

How to Run

Clone the repository:

git clone <your-repository-url>
cd <your-repository-name>

Install the required packages:

pip install pandas numpy matplotlib statsmodels

Confirm that AICPA_regressionAnalysisData.csv is stored in the same directory as the notebook.

Open and run Quiz_8.ipynb using Google Colab or Jupyter Notebook.

Key Takeaway

The analysis shows that seasonal conditions change how production translates into revenue. Allowing both the intercept and production slope to vary by season improves forecast accuracy and gives management a clearer view of H&S's monthly revenue patterns.
