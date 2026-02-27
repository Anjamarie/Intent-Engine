# Shopper Intent: Behavioral Psychology & Data Science
This project analyzes e-commerce session data to categorize shoppers based on their intent and psychological state. By transforming raw navigation data into behavioral indices, we can distinguish between "Mission Shoppers" and "Window Shoppers," and predict purchase probability through the lens of psychology.

## The Psychology of the Click
While traditional metrics focus on conversion rates, this project introduces three custom features designed to quantify user behavior:

  ### 1. Indecision Score
  Calculated by multiplying the average time spent on product-related pages by the session's exit rate.

  $$\text{Indecision Score} = \left( \frac{\text{Product Related Duration}}{\text{ProductRelated}} \right) \times \text{ExitRates}$$

  *Insight: High scores indicate a user who is stuck in "comparison paralysis"—spending significant time on products but showing a high probability of leaving the site.*

  ### 2. Impulsivity Index

  A ratio of total page engagement relative to the total duration of the session.

  $$\text{Impulsivity Index} = \left( \frac{\text{Admin+Info+Product Pages}}{\text{Total Duration}} \right)$$
  
​	
  *Insight: This measures "rapid-fire" browsing. High values suggest impulsive behavior or a high-efficiency user who knows exactly what they want.*
  

  ### 3. Goal Orientation

  A binary classification identifying users who skip "discovery" pages (Administrative or Informational) and go straight to product interactions.

  * Mission-Driven (1): High focus, low exploration.
  * Exploratory (0): High site engagement, often resulting in higher trust and conversion.


*** 

## Dataset Overview
The project utilizes the `online_shoppers_intention.csv` dataset, which contains 12,330 sessions:

  * **Administrative/Informational/ProductRelated:** Number of pages visited and time spent.
  * **Bounce & Exit Rates:** Metrics indicating site abandonment.
  * **PageValues:** The average value for a web page that a user visited before completing an e-commerce transaction.
  * **Revenue:** The target variable (True/False) indicating a successful purchase.

## Dataset Insights
The dataset contains 12,330 sessions. Initial analysis reveals:
  * Exploratory Browsers (Goal Orientation = 0) actually show a higher conversion rate (~21%) than pure mission shoppers.
  * High Indecision often correlates with specific VisitorType categories, allowing for targeted marketing interventions.


***


### Repository Structure
  * `ShopperIntent.ipynb`: Main analysis and feature engineering notebook.
  * `online_shoppers_intention.csv`: Raw session data containing 18 behavioral attributes.

    
### Technical Stack
  * Python 3.12
  * Pandas & NumPy: For feature engineering and data manipulation.
  * Jupyter Notebook: For interactive data exploration.

****

## Getting Started
Clone the repository:
```bash

git clone https://github.com/Anjamarie/Intent-Engine.git

```
Install dependencies:

```bash
pip install pandas numpy

```
**Run the analysis: Open `ShopperIntent.ipynb` to view the feature engineering and data distributions.**

### Feature Engineering Implementation
```python
import pandas as pd
import numpy as np

# Load the dataset
shopper_df = pd.read_csv('online_shoppers_intention.csv')

# 1. Calculate Indecision Score
shopper_df['indecision_score'] = (shopper_df['ProductRelated_Duration'] / 
                                  shopper_df['ProductRelated'].replace(0, 1)) * shopper_df['ExitRates']

# 2. Calculate Impulsivity Index
total_pages = shopper_df['Administrative'] + shopper_df['Informational'] + shopper_df['ProductRelated']
total_duration = (shopper_df['Administrative_Duration'] + 
                  shopper_df['Informational_Duration'] + 
                  shopper_df['ProductRelated_Duration']).replace(0, 1)
shopper_df['impulsivity_index'] = total_pages / total_duration

# 3. Define Goal Orientation
shopper_df['is_goal_oriented'] = ((shopper_df['Administrative'] == 0) & 
                                  (shopper_df['Informational'] == 0)).astype(int)
```

