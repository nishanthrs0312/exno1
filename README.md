# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
## Dataset 1 :  Loan_data.csv
```

import pandas as pd
import numpy as np
from google.colab import files
from io import BytesIO

uploaded = files.upload()
filename = next(iter(uploaded))
df = pd.read_csv(BytesIO(uploaded[filename]))

print("ORIGINAL DATASET")
print(df.head())
print("\nDataset Shape:", df.shape)

print("\nMissing Values Before Cleaning:")
print(df.isnull().sum())

num_cols = df.select_dtypes(include=np.number).columns

for col in num_cols:
    if df[col].isnull().any():
        df[col] = df[col].fillna(df[col].median())

cat_cols = df.select_dtypes(
    include=["object", "category"]
).columns

for col in cat_cols:
    if df[col].isnull().any():
        mode_values = df[col].mode()
        if not mode_values.empty:
            df[col] = df[col].fillna(mode_values[0])

df.drop_duplicates(inplace=True)

print("\nDATA CLEANING COMPLETED")
print("Missing Values After Cleaning:")
print(df.isnull().sum())
print("Dataset Shape After Cleaning:", df.shape)


num_cols = df.select_dtypes(include=np.number).columns
outlier_rows = pd.Series(False, index=df.index)

for col in num_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR

    column_outliers = (
        (df[col] < lower_bound) |
        (df[col] > upper_bound)
    )

    print(f"\nColumn: {col}")
    print("Lower Bound:", lower_bound)
    print("Upper Bound:", upper_bound)
    print("Number of Outliers:", column_outliers.sum())

    outlier_rows |= column_outliers

print("\nDETECTED OUTLIER ROWS")
print(df[outlier_rows])

df_cleaned = df.loc[~outlier_rows].copy()

print("\nDATASET AFTER OUTLIER REMOVAL")
print(df_cleaned.head())
print("Original Rows:", len(df))
print("Rows After Outlier Removal:", len(df_cleaned))
print("Outlier Rows Removed:", outlier_rows.sum())

output_file = "Loan_cleaned_without_outliers.csv"
df_cleaned.to_csv(output_file, index=False)

print("\nData Cleaning and Outlier Removal Completed Successfully!")

files.download(output_file)
```
## Output 1 : 
<img width="1916" height="1103" alt="image" src="https://github.com/user-attachments/assets/69688da5-9b21-4426-a028-9a960e805aba" />
<img width="1916" height="1155" alt="image" src="https://github.com/user-attachments/assets/ab1d5067-b918-4f06-8e1f-475b4dda8b06" />
<img width="1905" height="1108" alt="image" src="https://github.com/user-attachments/assets/bc862b90-3dfb-48bb-bbc7-cc1dd0e0e56a" />


## Dataset 2 : heights.csv : 

```
import pandas as pd
import numpy as np
from google.colab import files
from io import BytesIO

uploaded = files.upload()
filename = next(iter(uploaded))
df = pd.read_csv(BytesIO(uploaded[filename]))

print("ORIGINAL DATASET")
print(df.head())
print("\nDataset Shape:", df.shape)


print("\nMissing Values Before Cleaning:")
print(df.isnull().sum())

num_cols = df.select_dtypes(include=np.number).columns

for col in num_cols:
    if df[col].isnull().any():
        df[col] = df[col].fillna(df[col].median())

cat_cols = df.select_dtypes(
    include=["object", "category"]
).columns

for col in cat_cols:
    if df[col].isnull().any():
        mode_values = df[col].mode()
        if not mode_values.empty:
            df[col] = df[col].fillna(mode_values[0])

df.drop_duplicates(inplace=True)

print("\nDATA CLEANING COMPLETED")
print("Missing Values After Cleaning:")
print(df.isnull().sum())
print("Dataset Shape After Cleaning:", df.shape)

Q1 = df["height"].quantile(0.25)
Q3 = df["height"].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

print("\nQ1:", Q1)
print("Q3:", Q3)
print("IQR:", IQR)
print("Lower Bound:", lower_bound)
print("Upper Bound:", upper_bound)

outliers = df[
    (df["height"] < lower_bound) |
    (df["height"] > upper_bound)
]

print("\nDETECTED OUTLIERS")
print(outliers)

df_cleaned = df[
    (df["height"] >= lower_bound) &
    (df["height"] <= upper_bound)
].copy()

print("\nDATASET AFTER OUTLIER REMOVAL")
print(df_cleaned)

print("\nOriginal Rows:", len(df))
print("Rows After Removal:", len(df_cleaned))
print("Outliers Removed:", len(outliers))

output_file = "heights_cleaned_without_outliers.csv"

df_cleaned.to_csv(output_file, index=False)

print("\nData Cleaning and Outlier Removal Completed Successfully!")

files.download(output_file)

```
## Output 2 :
<img width="1917" height="1103" alt="image" src="https://github.com/user-attachments/assets/34a20a53-1a06-4b04-a487-50d98451b4fd" />




# Result

