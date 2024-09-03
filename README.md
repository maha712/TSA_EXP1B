# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 

Developed by: MAHALAKSHMI K

Register no: 212222240057

### AIM:

To perform regular differncing,seasonal adjustment and log transformatio on covid 2021  data

### ALGORITHM:

1. Import the required packages like pandas and numpy
3. Read the data using the panda
4. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
5. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
6. Display the overall results.

### PROGRAM:

import pandas as pd

import numpy as np

import matplotlib.pyplot as plt

from statsmodels.tsa.stattools import adfuller

# Load the dataset

file_path = '/content/Covid Data - Asia.csv'

data = pd.read_csv(file_path)

# Clean column names by stripping any extra spaces

data.columns = data.columns.str.strip()

# Rename the first column for easier access

data.rename(columns={'Country, Other': 'Country'}, inplace=True)

# Select a column to test for stationarity (e.g., 'Total Cases')

selected_column = 'Total Cases'

# Convert the selected column to a numeric format (remove commas)

data[selected_column] = data[selected_column].str.replace(',', '').astype(float)

# Function to check stationarity using the Augmented Dickey-Fuller test

def check_stationarity(timeseries):

    adf_test = adfuller(timeseries, autolag='AIC')
    
    print('ADF Statistic:', adf_test[0])
    
    print('p-value:', adf_test[1])
    
    for key, value in adf_test[4].items():
    
        print(f'Critical Value ({key}): {value}')
        
    return adf_test[1]  # Return the p-value

# Plot the original data

plt.figure(figsize=(10, 6))

plt.plot(data[selected_column], label=f'Original {selected_column}')

plt.title(f'Original {selected_column} Data')

plt.xlabel('Country Index')

plt.ylabel(selected_column)

plt.legend()

plt.show()

# Check stationarity of the original series

p_value = check_stationarity(data[selected_column])

print("p-value:", p_value)

# If the data is non-stationary, apply differencing

if p_value > 0.05:

    data[f'{selected_column}_diff'] = data[selected_column].diff().dropna()

    # Plot the differenced series
    
    plt.figure(figsize=(10, 6))
    
    plt.plot(data[f'{selected_column}_diff'], label=f'Differenced {selected_column}')
    
    plt.title(f'Differenced {selected_column} Data')
    
    plt.xlabel('Country Index')
    
    plt.ylabel(f'Differenced {selected_column}')
    
    plt.legend()
    
    plt.show()

    # Check stationarity of the differenced series
    
    p_value_diff = check_stationarity(data[f'{selected_column}_diff'].dropna())
    
    print("p-value after differencing:", p_value_diff)
    
    # If necessary, apply log transformation
    
    if p_value_diff > 0.05:
    
        data[f'{selected_column}_log'] = np.log(data[selected_column].replace(0, np.nan)).dropna()

        # Plot the log-transformed series
        
        plt.figure(figsize=(10, 6))
        
        plt.plot(data[f'{selected_column}_log'], label=f'Log Transformed {selected_column}')
        
        plt.title(f'Log Transformed {selected_column} Data')
        
        plt.xlabel('Country Index')
        
        plt.ylabel(f'Log {selected_column}')
        
        plt.legend()
        
        plt.show()

        # Check stationarity of the log-transformed series
        
        p_value_log = check_stationarity(data[f'{selected_column}_log'].dropna())
        
        print("p-value after log transformation:", p_value_log)
        
else:

    print(f"{selected_column} data is already stationary.")


### OUTPUT:


REGULAR DIFFERENCING:
![Screenshot (554)](https://github.com/user-attachments/assets/2e0cd4e5-49e9-48f2-8e9e-322da03c6661)


SEASONAL ADJUSTMENT:
![Screenshot (555)](https://github.com/user-attachments/assets/77eea774-37e9-4e40-93fb-bd6dd387052d)


LOG TRANSFORMATION:
![Screenshot (556)](https://github.com/user-attachments/assets/bba1aae0-41fb-462a-9b53-42cb68479137)



### RESULT:

Thus we have created the python code for the conversion of non stationary to stationary data 
