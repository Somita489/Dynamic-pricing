# Dynamic-pricing for Urban Parking Lots

Capstone Project — Summer Analytics 2025  
Hosted by Consulting & Analytics Club × Pathway  

---

##  Overview

This project simulates an intelligent, real-time parking pricing system for 14 urban parking spaces. Prices are dynamically adjusted based on demand drivers such as occupancy, vehicle type, queue length, special events, and nearby traffic.  

The pipeline includes:

- **Data exploration & preprocessing**
- **Baseline and demand-driven pricing models**
- **Real-time data streaming using Pathway**
- **Interactive visualizations with Bokeh**

---

##  Project Workflow

### Setup
- Install dependencies (Bokeh, Pathway):
  ```python
  !pip install bokeh pathway
  import numpy as np
  import pandas as pd
  import matplotlib.pyplot as plt
  import datetime
  from datetime import datetime
  import pathway as pw
  import bokeh.plotting
  import panel as pn

  
### Data Exploration and preprocessing
- Understand data shape and schema:
- Inspect .shape and .info().
- Count unique values per column to identify categorical features.
- Encode string values
    - Vehicle types - integers
    - Traffic congestion levels - integers
- Handle any missing or inconsistent data


--

## Model 1
- This step establishes the foundation for Model 1, a simple linear pricing model to validate the core idea before adding complexity.

  - For each record, calculate the ratio of occupied spots to total capacity:
  - Define the Linear Pricing Function:
    ```python
    def linear_model_model1(df, alpha=1.0, base_price=10):
    prices = []
    for i in range(len(df)):
        new_price = base_price + alpha * df.iloc[i]['occupancy_ratio']
        prices.append(new_price)
    df['price_model'] = prices
    return df
  - alpha: Controls the sensitivity of the price to occupancy
- Verified the price remains in a reasonable range ($10–$11.6).
- Generated a line plot showing price increases linearly as occupancy increases, confirming expected behavior.
  ![image](https://github.com/user-attachments/assets/a9fe9527-3f48-4f21-8c68-4ad123037fe4)

  
---


## Model 2
- This step builds a more realistic pricing model incorporating multiple demand drivers to better reflect dynamic parking conditions such as :
    - Occupancy ratio
    - Vehicle type
    - Queue length
    - Special days
    - Traffic congestion
      ```python
      def model2(dataset, a, b, c, d, e, min_target=5, max_target=20, base_price=10.0):
      # Compute weighted raw demand
      raw_demand = (a * occupancy) + (b * vehicle_type) + ...
      # Convert to price
      price = base_price * (1 + raw_demand)
      # Normalize to target range
      normalized_price = normalize_to_range(price)
      return dataset
- Normalization: Rescales prices to target range(5,20) for consistency
#### Determining weights
- Initially run the model with initialweight guesses
- Then plot normalized prices against each feature to evaluate and fine-tune weights
- Values obtained :
    - a=0.1
    - b=4.0
    - c=0.4
    - d=0.6
    - e=2.5
 
      
      ![image](https://github.com/user-attachments/assets/b4b303f9-1061-4392-9504-77015c40e74e)
      ![image](https://github.com/user-attachments/assets/fe014813-0905-4b66-88ab-3e810accd98a)


---


## Real time streaming with Pathway
- Simulates live data ingestion:
  -Stream interval: 1 second
- Resembles a near real-time parking scenario.
- Dynamic pricing predictions are emitted continuously.
- Based on sample Pathway notebook from Analytics Club.


### Interactive Visualization
- Real-time Bokeh plots:
- Live pricing curves
- Feature correlations
- Comparison across lots
![image](https://github.com/user-attachments/assets/007a7dfa-c440-447d-a3bf-3b5be7ae1e57)
![image](https://github.com/user-attachments/assets/34ccb204-bc88-4ea4-95f6-bde474f0c1be)


  


  








