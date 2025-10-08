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
```
import pandas as pd
import numpy as np
from scipy import stats

def initial_data_info(df):
    print("Info:")
    print(df.info())
    print("\nDescribe:")
    print(df.describe())
    print("\nNull values per column:")
    print(df.isnull().sum())
    print('=' * 40)

def remove_nulls(df):
    return df.dropna()

def remove_outliers_iqr(df, columns):
    for col in columns:
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        df = df[(df[col] >= lower) & (df[col] <= upper)]
    return df

def remove_outliers_zscore(df, columns, thresh=3):
    for col in columns:
        z = np.abs(stats.zscore(df[col].dropna()))
        filtered_entries = (z < thresh)
        valid_indices = df[col].dropna().index[filtered_entries]
        df = df.loc[valid_indices]
    return df

file_names = [
    "Data_set.csv",
    "heights.csv",
    "iris.csv",
    "Loan_data.csv",
    "SAMPLEIDS.csv"
]

for file in file_names:
    print(f'Processing file: {file}')
    df = pd.read_csv(file)
    
    print("Initial Info")
    initial_data_info(df)
    
    df_no_nulls = remove_nulls(df)
    print("After Removing Nulls")
    print(df_no_nulls.shape)
    
    numeric_cols = df_no_nulls.select_dtypes(include=[np.number]).columns.tolist()
    
    df_iqr = remove_outliers_iqr(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (IQR method):", df_iqr.shape)
    
    df_z = remove_outliers_zscore(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (Z-score method):", df_z.shape)
    print('=' * 80)
```                
# Result
<img width="475" height="712" alt="Screenshot 2025-10-08 094830" src="https://github.com/user-attachments/assets/bbf7f3c8-b006-4c65-b555-332d41c462bc" />
<img width="676" height="292" alt="Screenshot 2025-10-08 094841" src="https://github.com/user-attachments/assets/f8e2c94e-b56e-42f5-9a25-478c5053c965" />
<img width="655" height="609" alt="Screenshot 2025-10-08 094858" src="https://github.com/user-attachments/assets/76c1e5aa-2f0d-4627-bb21-316a6afe9d5f" />
<img width="650" height="700" alt="Screenshot 2025-10-08 094910" src="https://github.com/user-attachments/assets/0d4cfa90-36ae-43af-bced-38bc47eaea9f" />
<img width="633" height="759" alt="Screenshot 2025-10-08 094920" src="https://github.com/user-attachments/assets/9b55a924-4c37-46a1-b0a4-6cdaaee55812" />
<img width="649" height="346" alt="Screenshot 2025-10-08 094929" src="https://github.com/user-attachments/assets/462597a2-c48c-4fac-9538-aebf4639a99f" />
<img width="681" height="771" alt="Screenshot 2025-10-08 094945" src="https://github.com/user-attachments/assets/d09760bb-6223-4556-85a5-dd15c94b700c" />
<img width="649" height="391" alt="Screenshot 2025-10-08 094952" src="https://github.com/user-attachments/assets/14028ee1-2e72-48e3-b150-9c19f44f0184" />


         
