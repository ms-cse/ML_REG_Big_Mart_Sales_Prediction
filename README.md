## Machine Learning Regression - Big Mart Sales Prediction


## A. Data Assessment Process   
### A1. Original Dataset Summary:
- Total number of records: 8523
- Total number of features: 12
- Numeric type features: 5 **['Item_Weight', 'Item_Visibility', 'Item_MRP', 'Item_Outlet_Sales', 'Outlet_Establishment_Year']**
- Categorical type features: 7 **['Item_Identifier', 'Item_Fat_Content', 'Item_Type', 'Outlet_Identifier', 'Outlet_Size', 'Outlet_Location_Type', 'Outlet_Type']**
- For machine learning applications **'Item_Outlet_Sales'** feature is the target variable (dependent) and the other 11 features are input (independent).

### A2. Features Description:
| S.NO | Feature                   | Description                                                                  |
|:----:| :---                      | :---                                                                         |
| 1.   | Item_Identifier           | Unique identifier number assigned to each item                               |
| 2.   | Item_Weight               | Item weight in gms                                                           |
| 3.   | Item_Fat_Content          | Item fat content                                                             |
| 4.   | Item_Visibility           | Placement value of each item: (0 - 'Far & Behind') and (1 - 'Near & Front')  |
| 5.   | Item_Type                 | Type of food category item belongs to                                        |
| 6.   | Item_MRP                  | Max. Retail Price of the item in the outlet                                  |
| 7.   | Outlet_Identifier         | Unique outlet identifier                                                     |
| 8.   | Outlet_Establishment_Year | Year of outlet establishment                                                 |
| 9.   | Outlet_Size               | Size of the outlet                                                           |
| 10.  | Outlet_Location_Type      | Tier of city in which outlet is located                                      |
| 11.  | Outlet_Type               | Type of outlet                                                               |
| 12.  | Item_Outlet_Sales         | Sales of the item from the outlet                                            |
|      |                           |                                                                              |

### A3. Data Issues:
#### a. Dirty Data (Low quality):
- Completeness: Missing values for 'Item_Weight'=1463, and 'Outlet_Size'=2410.
  
- Validity: No duplicate observations.

- Accuracy: No inaccuracy issues.

- Consistency:
  - "Item_Fat_Content" feature values to be marked as 'Non Edible' where the context is 'Non Consumables'.
  - "Item_Fat_Content" feature values 'LF','low fat' must be mapped to 'Low Fat' and 'reg' must be mapped to 'Regular'.
  - "Item_Type" feature values to be marked as 'DR_Dairy' and 'FD_Dairy', where the context is 'Drinks' and 'Foods' respectively.

#### b. Messy Data (Untidy / Structural):
- No structure related issues.


## B. Data Pre-Processing Results
- Data splitting in Train, Validation, and Test sets.
- Creating a high level item category as 'Item_Category' using 'Item_Identifier' feature by selecting first two letters of the feature values. (FD, DR, NC) ---> (Foods, Drinks, Non Consumables).
- Imputing missing values for the Features.
- Marking the 'Item_Fat_Content' value as 'Non Edible', where 'Item_Category' is 'Non Consumables'.
- Remapping the feature values of "Item_Fat_Content" ('LF','low fat') to 'Low Fat', 'reg' to 'Regular'.
- Marking the 'Item_Type' as 'Dairy Drinks' and 'Dairy Foods' where the 'Item_Category' is 'Drinks' and 'Foods' respectively.
- Creating new feature 'Outlet_Age' from 'Outlet_Establishment_Year' by expression = 2013-year.
- Correcting 'Item_Visibility' if 0, replace with mean.
- Finally, the data sets are saved in the CSV and PKL files for further analysis and ML modelling.


## C. Conclusions / Insights of Exploratory Data Analysis
- Some features have highly skewed distribution i.e., features are not Normally Distributed.
- Outliers vary in the range of (0-2.18) % for the features.
- 'Item_Identifier' and 'Oultlet_Identifier' features can be dropped for model buidling.
- 'Item_Type' has lot of unique categorical values, needs to be handled.
- 'Outlet_Establishment_Year' feature is used to create new feature 'Outlet_Age'. So, can be dropped for model building.
- Numerical features exhibit a low to moderate correlation among them.
- Foods Item are contributing more for sales than Drinks and Non Consumables.
- Regular Fat items are giving more sales than Low Fat and Non Edible. But more units are sold for Low Fat items.
- Medium size outlets have more sales, but more items are sold in Small size outlets.
- Highest sale is in Tier 2 location outlets, but more units are sold in Tier 3 location types.
- Foods and Drinks with Regular and Low Fat respectively are giving more sales.
- Foods, Drinks, Non Consumables with Seafood, Hard Drinks, and Households are contributing more for sales.
- Items in each category have highest sales in Medium size, Tier 2, Supermarket Type 3 outlets.
- Regular Fat items are contributing more for sales, except for few Low Fat Items.
- Sale of Grocery Store is lower in all location types.


## D. Feature Engineering
- Drop non relevant features from the observations.
- Oultier Detection and Handling using Capping Technique.
- Feature Transformation using 'Yeo-Johnson' (to be applied in pipeline).
- Scaling (StandardScaler) (to be applied in pipeline).
- Categorical features encoding using Ordinal encoding and OneHot encoding techniques (to be applied in pipeline).
- Feature Selection SelectKBest with mutual_info_regression techniques (to be applied in pipeline).
- Treat Train, Validation, and Test dataset with feature selection strategy.


## E. Model Building and Hyper Parameter Tuning
- List of models compared: **[Linear Regression, Lasso, Ridge, KNeighborsRegresor, SVR, DecisionTreeRegressor, BaggingReggressor, RandomForestRegressor, GradientBoostingRegressor, HistGradientBoostingRegressor, XGBRegressor]**.
- Hyper-Parameter Tuning: GridSearchCV with KFold using ML pipelines.


## F. Best Model Results
- **RandomForestRegressor (bootstrap=True, criterion='squared_error', max_depth=5, max_samples=0.25, n_estimators=100, oob_score=True, random_state=46)**
- KFold(n_splits=5, shuffle=True, random_state=46)
  - Mean R2 Score: 0.58849, Mean RMSE Score: 1094.27541, dataset size : (8323, 11)
- Validation Data : Mean R2 Score: 0.6415, Mean RMSE Score: 892.7377, dataset size : (100,11)
- Test Data : Mean R2 Score: 0.5689, Mean RMSE Score: 1232.2241, dataset size : (100,11)


## G. Production Model
- **RandomForestRegressor (bootstrap=True, criterion='squared_error', max_depth=5, max_samples=0.25, n_estimators=100, oob_score=True, random_state=46)**
- KFold(n_splits=10, shuffle=True, random_state=46), Mean R2 Score: 0.5884, Mean RMSE Score: 1092.5346, dataset size : (8423, 11)
- Test Data : Mean R2 Score: 0.5693, Mean RMSE Score: 1231.6046, dataset size : (100,11)


## H. Gradio App Development  
- Available under 'GR_APP' folder.
- Gradio app inside the Jupyter Notebook file
- App accepts the inputs from the user through the UI interface, performs the logic using production model, and displays the prediction as output.
- The user can try out some examples to see the app's working.
