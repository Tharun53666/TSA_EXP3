# Ex.No: 03   COMPUTE THE AUTO FUNCTION(ACF)
Date: 02-05-2026

### AIM:
To Compute the AutoCorrelation Function (ACF) of the data for the first 35 lags to determine the model
type to fit the data.
### ALGORITHM:
1. Import the necessary packages
2. Find the mean, variance and then implement normalization for the data.
3. Implement the correlation using necessary logic and obtain the results
4. Store the results in an array
5. Represent the result in graphical representation as given below.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# ── Load dataset and extract monthly avg Volume for HDFCBANK.NS ───────────────
STOCK    = 'HDFCBANK.NS'
CSV_PATH = 'C:/Users/admin/Downloads/nifty50_2000_2025.csv'   # keep CSV in same folder as notebook

df = pd.read_csv(CSV_PATH)
df['Date'] = pd.to_datetime(df['Date'])

stock_df = df[df['Stock'] == STOCK].sort_values('Date').copy()
stock_df['YearMonth'] = stock_df['Date'].dt.to_period('M')

monthly = stock_df.groupby('YearMonth')['Volume'].mean().reset_index()

# Replace hardcoded data list with dataset values (same structure as original)
data = monthly['Volume'].tolist()

print(f"Stock       : {STOCK}")
print(f"Data points : {len(data)}  (monthly avg volume from 2000–2024)")

lags = range(35)

# Pre-allocate autocorrelation table
autocorr_values = []

# Mean
mean_data = np.mean(data)

# Variance
variance_data = np.var(data)

# Normalized data
N = len(data)

# Go through lag components one-by-one
for lag in lags:
    if lag == 0:
        autocorr_values.append(1)
    else:
        auto_cov = np.sum(
            (np.array(data[:-lag]) - mean_data) *
            (np.array(data[lag:])  - mean_data)
        ) / N
        autocorr_values.append(auto_cov / variance_data)

# Display the graph
plt.figure(figsize=(12, 5))
plt.stem(lags, autocorr_values)
plt.axhline(y=0, color='black', linewidth=0.8)
plt.title(f'Autocorrelation of {STOCK} — Monthly Avg Volume')
plt.xlabel('Lag (months)')
plt.ylabel('Autocorrelation')
plt.legend()
plt.grid(True)
plt.show()
```

### OUTPUT:
<img width="1468" height="742" alt="image" src="https://github.com/user-attachments/assets/0005476f-2c5f-420d-96fa-6a94f1e1b067" />


### RESULT:
        Thus we have successfully implemented the auto correlation function in python.
