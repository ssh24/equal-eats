
## Investigate Raw Data
### Author: S. Sakib Hasan
#### Date: March 20, 2020 @ 21:03 EDT

#### Set up environment


```python
import os
import pandas as pd
import numpy as np
```


```python
scripts_dir = os.getcwd();
```


```python
os.chdir(os.path.join(scripts_dir, "..", "data"));
```


```python
data_dir = os.getcwd();
```


```python
rawFileName = "raw-data.xlsx";
```


```python
df = pd.read_excel(rawFileName);
```

#### Get the shape of the data


```python
df.shape
```




    (3424, 20)



#### Get the column names


```python
df.columns
```




    Index(['First Name (Billing)', 'Last Name (Billing)', 'Company (Billing)',
           'Address 1&2 (Billing)', 'City (Billing)', 'State Code (Billing)',
           'Postcode (Billing)', 'Country Code (Billing)', 'Email (Billing)',
           'Phone (Billing)', 'First Name (Shipping)',
           'Summary Report Total Orders', 'Summary Report Total Items',
           'Summary Report Total Items (Exported)', 'Summary Report Total Amount',
           'Summary Report Total Amount Paid', 'Summary Report Total Shipping',
           'Summary Report Total Discount', 'Summary Report Total Refunds',
           'Summary Report Total Refund Amount'],
          dtype='object')



#### Describe the dataframe


```python
df.describe().T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>First Name (Shipping)</th>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Summary Report Total Orders</th>
      <td>3424.0</td>
      <td>1.158586</td>
      <td>0.809830</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Items</th>
      <td>3424.0</td>
      <td>1.682827</td>
      <td>1.329188</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Items (Exported)</th>
      <td>3424.0</td>
      <td>1.682827</td>
      <td>1.329188</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Amount</th>
      <td>3424.0</td>
      <td>13.553446</td>
      <td>10.597160</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>16.0</td>
      <td>168.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Amount Paid</th>
      <td>3424.0</td>
      <td>10.504381</td>
      <td>11.267106</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>16.0</td>
      <td>120.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Shipping</th>
      <td>3424.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Discount</th>
      <td>3424.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Refunds</th>
      <td>3424.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Summary Report Total Refund Amount</th>
      <td>3424.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Get the number of null's in each column


```python
df.isnull().sum()
```




    First Name (Billing)                      926
    Last Name (Billing)                      1154
    Company (Billing)                        3247
    Address 1&2 (Billing)                     926
    City (Billing)                            926
    State Code (Billing)                     1091
    Postcode (Billing)                        934
    Country Code (Billing)                    931
    Email (Billing)                             1
    Phone (Billing)                           928
    First Name (Shipping)                    3424
    Summary Report Total Orders                 0
    Summary Report Total Items                  0
    Summary Report Total Items (Exported)       0
    Summary Report Total Amount                 0
    Summary Report Total Amount Paid            0
    Summary Report Total Shipping               0
    Summary Report Total Discount               0
    Summary Report Total Refunds                0
    Summary Report Total Refund Amount          0
    dtype: int64




```python
df[df["Email (Billing)"].isna()]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First Name (Billing)</th>
      <th>Last Name (Billing)</th>
      <th>Company (Billing)</th>
      <th>Address 1&amp;2 (Billing)</th>
      <th>City (Billing)</th>
      <th>State Code (Billing)</th>
      <th>Postcode (Billing)</th>
      <th>Country Code (Billing)</th>
      <th>Email (Billing)</th>
      <th>Phone (Billing)</th>
      <th>First Name (Shipping)</th>
      <th>Summary Report Total Orders</th>
      <th>Summary Report Total Items</th>
      <th>Summary Report Total Items (Exported)</th>
      <th>Summary Report Total Amount</th>
      <th>Summary Report Total Amount Paid</th>
      <th>Summary Report Total Shipping</th>
      <th>Summary Report Total Discount</th>
      <th>Summary Report Total Refunds</th>
      <th>Summary Report Total Refund Amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>350</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40</td>
      <td>3</td>
      <td>3</td>
      <td>24</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>


