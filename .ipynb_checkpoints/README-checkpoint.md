
#  General Assembly Data Science Final Project: Yacht Insurance Claims
#### By: Carly Sharma

### Problem Statement
The insurance industry relies on data to inform premium costs for current and future clients. Yacht Insurance is a niche part of the industry historically known for being a profitable, yet small book of business within larger insurance portfolios (like commercial marine insurance). Because of its size and general dependability, its profitbility was often not thoroughly evaluated. However, more recently, there has been a trend to dive more deeply into past data to ensure yacht insurance can remain or become profitable. 

This project looks at real world data from a yacht insurance broker based in the U.S. One dataset looks at all the claims that have occurred from 2016 - 2020. The other dataset includes all policies in the company's portfolio from the same date range along with various features about the boat and boat owner (like the builder of the boat or the age of the owner). This is binary classification problem and our models predicted whether or not a policy is likely to have a least one claim in five years. Because we were mostly interested in classifying claims (the minority class) we looked at recall as our metric. We tested Logistic Regression, K-Nearest Neighbors, Random Forest, and Extra Trees models with various combinations of over and undersampling.


---
### Background Research
Given our client has been in the yacht insurance industry for 40 years, he was our main source of research for any questions about the industry or dataset.

---
### Contents
* **01_Data_Cleaning:** Data imports, cleaning, and merging into a dataframe for modeling
* **02_Model_Preprocessing:** Model preprocessing, feature engineering, EDA
* **03_Modeling_and_Results:** Models, over/undersampling, results

---
### Datasets Used

* `Claims_report.xlsx`: Client provided spreadsheet of claims from 2016-2020.
* `NYP Bordereau Report - Total.xlsx - Bordereau Report.csv`: Client provided spreadsheet of all polcies and attributes from 2016-2020.
* `combined.csv`: Cleaned and merged dataset of the two datasets provided.
* `combined2.csv`: The combined dataset that was prerprocessed for modeling.

---

### Data Dictionary
|Feature|Type|Dataset|Description|
|---|---|---|---|
|**New/Renl/Endt/Canc/Flat**|*object*|`combined.csv`|Whether or not a policy is new, a renewal, an endorsement, cancellation, or flat.|
|**Married yes/no**|*object*|`combined.csv`|Whether or not the owner of the policy is married.|
|**Years Exp.**|*float*|`combined.csv`|Self reported years of boating experience of the policy holder.|
|**Occupation**|*object*|`combined.csv`|The occupation of the polcy holder.|
|**Builder**|*object*|`combined.csv`|The builder/maker of the boat (aka the brand).|
|**Year Built**|*float*|`combined.csv`|The year the boat was built.|
|**Construction**|*object*|`combined.csv`|What material the boat was mdae out of.|
|**Length**|*float*|`combined.csv`|The length of the boat in feet.|
|**Hull Type**|*object*|`combined.csv`|If the boat is monohull, multihull, etc.|
|**Hull Limit**|*float*|`combined.csv`|The cost of the boat in USD.|
|**Power Type**|*object*|`combined.csv`|How the engines are configured (inboard, outboard, pod, i/o)
|**# Engines**|*float*|`combined.csv`|How many engines the boat has.|
|**Mooring County**|*object*|`combined.csv`|Which county the boat is kept/moored in.|
|**num_claims**|*float*|`combined.csv`|How many claims a policy has.|
|**Age**|*int*|`combined.csv`|How old the policy owner is.|
|**policy_length**|*float*|`combined.csv`|How long someone has a policy. Subtracted from today's date if it is ongoing.|


---
### Analysis Summary
#### Notebook 1
We began with two separate datasets, one for claims, and one for all policies. We knew we would eventually be merging them using their policy number columns. From the outset it was clear that our target variable, or, what we were trying to predict, had very little data. These were all the claims in the claims dataset and it made up roughly 6.5% of the total policies we had. From the outset so we had to do whatever we could to retain these rows of data. Some strategies we implemented were:
1. Asking the client for updated numbers that had errors (like in the Date of Birth or Construction columns).
2. Did outside searching to fill in null values (like using ZIP code to fill in missing county columns).
3. Instead of dropping rows for missing values in Occupation and Married columns we input "not reported".

**Working with real world data**
Because this was real data from a real company, there were a decent amount of human errors to clean in both datasets. Some examples were:
1.  Values written in both uppercase and lowercase letters. *We lowercased everything.*
2.  Special characters found in location data like bcs or b.c.s. *We removed special characters where necessary.*
#### Notebook 2
Once the two dataframes were merged, we did additional cleaning to get the data ready for modeling like filling in any remaining null values. We then engineered columns we thought would be helpful in predicting the likelihood of having a claim: 'Age', 'Policy Length', and 'Number of Claims'. We looked at datatypes for all of our columns and transformed any that needed transformation. Additionally, we looked at outliers and correlation between columns to see if any should be dropped. For all remaining categorical columns we dummified them in order to include in our model. Finally, we looked at our remaining columns and dropped redundant or unnecessary columns.
#### Notebook 3 
We initially tested 4 multiclass classification models with the target variable being the number of claims a policy has. Below were the number of rows in each targeted class:
* **0 claims:** 5836
* **1 claim:** 421
* **2 claims:** 68
* **3 claims:** 15
*After testing this problem using Logistic Regression, K-Nearest Neighbors, a Random Forest Classifier, and Extra Trees, we realized there was just too little data to continue down this path.*

We ultimately tested 4 binary classification models: Logistic Regression, K-Nearest Neighbors, a Random Forest Classifier, and Extra Trees. This changed our target variable to be whether a policy has had *at least* one claim. It broke down to:
* 0 claims: 92% of the data
* 1 or more claims: 8% of the data

**Metrics:** Because we were interested in predicting claims, the minority class, we decided to use recall as our target metric. We decided we would rather wrongly predict a boat would have a claim and it not than wrongly predict a boat would not have a claim and it have one. 

###### Step 1.
Instantiate and fit 4 models.
###### Step 2.
Test the same models with oversampling the minority class.
###### Step 3. 
Test the same models with a combination of oversampling the minority class, then undersampling the majority class.
###### Step 4. 
Test the same models with a combination of oversampling the minority class using SMOTE, then undersampling the majority class.

**The best combination ended up being the final combination of SMOTE and undersampling. The best model was Random Forest.**
**(eventually insert table of results)**

From there, we fine tuned the models through:
* Feature selection using feature importance
* Gridsearch to find the best parameters
* Feature selection to reduce overfitting the model

---

### Conclusions & Recommendations
* Our best scoring model, Random Forest, had a recall score of 45%. This means that of all the policies that eventually have a claim, our model predicts 45% of them accurately. 
* The features that were most important to predicting whether or not a policy has a claim were: **insert  feature importance table**
* We would recommend understanding why higher priced boats and older clients, tend to be more likely to have claims. Additionally, Monroe, FL and the Caribbean were the locations most highly correlated to predicting a claim. This is likely due to hurricanes and we would recommend charging higher premiums for these areas.



#### Further Research
If we could work more in depth with the team at the client's business we would want to clean the data from their internal system in the following ways:
1. Categorize Occupations
2. Categorize Builders
3. Make sure county data is as clean as possible (or categorize them)

If we had access to a larger claims dataset (like from GEICO for example) we would like to do a more in depth look into the types of claims/accidents reported. Weather, theft, etc.

