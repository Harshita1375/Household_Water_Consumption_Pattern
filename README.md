# House Hold Water Consumption Pattern
Water scarcity is an increasingly global issue, with urban households playing a major role in water wastage due to inefficient consumption habits. The goal of this project is to develop a Machine Learning model that predicts daily water consumption for individual households based on historical usage patterns, household characteristics, weather conditions, and conservation behaviors.

## Dataset Description
The dataset folder contains the following files: 
<ul>
<li>train.csv: 14000 x 12</li>
<li>test.csv: 6000 x 11</li>
</ul>

<table>
<tr>
<th>Column name</th>
<th>Description</th>
</tr>
<tr>
<td>Timestamp</td>
<td>Represents a unique timestamp of an entry</td>
</tr>
<tr>
<td>Residents</td>
<td>Represents the number of people living in the household</td>
</tr>
<tr>
<td>Timestamp</td>
<td>Represents a unique timestamp of an entry</td>
</tr>
<tr>
<td>Apartment_Type</td>
<td>Represents the type of apartment</td>
</tr>
<tr>
<td>Temperature</td>
<td>Represents the average temperature of that time period</td>
</tr>
<tr>
<td>Humidity</td>
<td>Represents the average humidity of that time period</td>
</tr>
<tr>
<td>Water_Price</td>
<td>Represents the water price for that time period</td>
</tr>
<tr>
<td>Period_Consumption_Index</td>
<td>Represents the relative water usage for each 8-hour period</td>
</tr>
<tr>
<td>Income_Level</td>
<td>Represents the income level of household</td>
</tr>
<tr>
<td>Guests</td>
<td>Represents the number of guests</td>
</tr>
<tr>
<td>Amenities</td>
<td>Represents the types of amenities available in the household</td>
</tr>
<tr>
<td>Appliance_Usage</td>
<td>Represents whether water appliances are being used or not </td>
</tr>
<tr>
<td>Water_Consumption	</td>
<td>Represents the consumption of water in that period</td>
</tr>
</table>

### Output Submission File:
- **File:** `Submission.csv`  
- **Shape:** `6000 x 2`  

---
## Data Pre-processing

### Handling Null Values:
- **Columns with Null Values:**
- `Apartment_Type` and `Income_Level`:
  - Both are categorical columns.  
  - Replaced `null` values with `'NA'`.  
- `Temperature` and `Appliance_Usage`:  
  - Both are numerical columns.  
  - Filled `null` values with the **mean** of their respective columns.  

### Handling Negative Values:
- **Columns with Negative Values:**  
  - `Residents`, `Water_Price`, and `Guests` contain null and negative values.  
  - Applied a **lambda function** to replace negative values with the **mean of all positive values** in their respective columns.  

### Saving the Pre-processed Data:
- The pre-processing results are saved for further predictions.

## Model Overview:

### Stacked Ensemble Model:
- I combined predictions from RandomForestRegressor and XGBRegressor into a meta-model using Linear Regression.
- The meta-model learns how to blend the individual predictions, aiming to reduce the overall error.

### Model Components:

### RandomForestRegressor:
- An ensemble learning algorithm that uses multiple decision trees to improve prediction accuracy.
- You tune the `n_estimators` parameter (number of trees).

### XGBRegressor:
- A gradient boosting algorithm that handles structured data efficiently.
- You optimize the `n_estimators` parameter for better generalization.

## Training Workflow:
1. **Model Training:**
   - Both models are trained on the training data `(X_train, y_train)`.
2. **Stacking:**
   - Their predictions are combined into stacked features.
3. **Meta-Model:**
   - A `Linear Regression` meta-model fits on the stacked predictions to make the final forecast.



## DEAP Genetic Algorithm for Hyperparameter Optimization

### Fitness Evaluation:
- The `evaluate()` function takes the hyperparameters of the models (number of estimators) as inputs.
- It:
  - Trains the models.
  - Makes predictions.
  - Calculates the **Root Mean Squared Error (RMSE)** on the test set.
- The GA minimizes the RMSE, improving the model’s accuracy.

### DEAP Configuration:
- **Population Size:** `POP_SIZE = 12`  
  - The GA starts with a population of 12 individuals (each individual represents a pair of hyperparameters: RF and XGB estimators).
- **Generations:** `GEN_COUNT = 36`  
  - The algorithm evolves over 36 generations, refining the model’s performance.

### Crossover & Mutation:
- **Crossover:**  
  - Uses `cxBlend` with `alpha=0.5`  
  - Combines the genetic material of two parents to create new individuals.
- **Mutation:**  
  - Uses `mutGaussian` with `mu=0` and `sigma=10`  
  - Introduces small changes in the hyperparameters to prevent local minima.

### Selection:
- Uses `selTournament` with `tournsize=3`  
  - Selects individuals for the next generation based on their performance.

### Optimization Outcome:
- After running the Genetic Algorithm (GA), the best individual (set of hyperparameters) is selected based on the lowest RMSE.
- The optimized hyperparameters (RF Estimators and XGB Estimators) are printed as the final result, providing the best combination for accurate water consumption prediction.

In the end model is saved(.pkl) for future use. 
<br>
Pre-processed test.csv was provided to model to predict water_consumption, then predictions and Timestamp is saved to <strong>submission.csv</strong>.

---
## Thank You

