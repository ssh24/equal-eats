
## Data Preparation
### Author: S. Sakib Hasan
#### Date: March 24, 2020 @ 20:50 EDT

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

#### Delete the customers who don't have email


```python
print("Shape before dropping: ", df.shape)
```

    Shape before dropping:  (3424, 20)



```python
df = df[df["Email (Billing)"].notnull()]
```


```python
print("Shape after dropping: ", df.shape)
```

    Shape after dropping:  (3423, 20)


#### Keep specific columns only


```python
cols_to_keep = ["Email (Billing)",
                "First Name (Billing)",
                "First Name (Shipping)",
                "Last Name (Billing)",
                "City (Billing)", 
                "Country Code (Billing)", 
                "State Code (Billing)", 
                "Summary Report Total Amount"]
```

#### Process to follow:
1. Filter the data with the respective columns above
2. Check if First Name (Billing) exists
  - If it exists, keep as is, move to step 3
  - If it does not exist, check if First Name (Shipping) exists
    - If it exists, use it as First Name, move to step 3
    - If it does not exist, move to step 3
3. Check if Last Name (Billing) exists
  - If it exists, keep as is
  - If it does not exist, move to step 4


```python
filtered_df = df[cols_to_keep]
```


```python
print("Shape of filtered dataframe: ", filtered_df.shape)
```

    Shape of filtered dataframe:  (3423, 8)


#### Apply process mentioned above to find customer first name


```python
def apply(row):
    firstName = row["First Name (Billing)"];
    if (firstName is None):
        if (row["First Name (Shipping)"] is not None):
            firstName = row["First Name (Shipping)"];
    return firstName;
```


```python
filtered_df["First Name"] = filtered_df.apply(apply, axis=1)
```

    /usr/local/lib/python3.7/site-packages/ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """Entry point for launching an IPython kernel.



```python
filtered_df.drop(["First Name (Billing)", "First Name (Shipping)"], axis=1, inplace=True);
```

    /usr/local/lib/python3.7/site-packages/pandas/core/frame.py:3930: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      errors=errors)


#### How many customers have either missing first name or last name


```python
filtered_df[(filtered_df["First Name"].isna()) | (filtered_df["Last Name (Billing)"].isna())].shape[0]
```




    1153



#### Filter out all the customers that have missing first name or last name (optional)


```python
filtered_df = filtered_df[(filtered_df["First Name"].notnull())]
filtered_df = filtered_df[(filtered_df["Last Name (Billing)"].notnull())]
```


```python
filtered_df.shape
```




    (2270, 7)



#### Rename and reorder columns


```python
renameKey = {}
for col in list(filtered_df.columns):
    if col.find("(Billing)") == -1:
        key = col;
    else:
        key = "".join(col.split("(Billing)")[:-1]).strip();
        
    renameKey[col] = key
```


```python
filtered_df.rename(columns=renameKey, inplace=True)
```


```python
new_cols = list(filtered_df.columns);
new_cols.insert(1, new_cols.pop())
```


```python
filtered_df = filtered_df[new_cols]
```

#### Take the first 1000 rows and save the data


```python
instances_to_take = 1000;
```


```python
customers = filtered_df.head(instances_to_take);
```


```python
print("Total numbers of customer data to be saved: {}".format(customers.shape[0]))
```

    Total numbers of customer data to be saved: 1000


#### Add a customized "date added" column
More on why this column added: https://help.activecampaign.com/hc/en-us/articles/115001050290-Import-contacts-from-a-Google-Sheet#import-frequency-and-adding-new-data


```python
from datetime import date
customers["Date Added"] = date.today().strftime("%d/%m/%Y");
```

    /usr/local/lib/python3.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      



```python
customers.to_excel("customers.xlsx", index=False);
```
