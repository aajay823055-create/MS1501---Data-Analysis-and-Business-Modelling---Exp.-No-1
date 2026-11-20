# Experiment No. 1: Data Cleaning and Preprocessing

## Aim

To perform data cleaning and preprocessing on a business dataset using Python libraries for better data quality and analysis readiness.

## Dataset

The dataset used for this experiment is `heartdisease.csv`. It contains
medical and demographic information related to heart disease patients.

The dataset is opened and analyzed using Jupyter Notebook to study its
data structure, identify data types, and perform basic statistical
analysis for data-cleaning and preprocessing purposes.

## Algorithm

1.  Import essential Python libraries.
2.  Load the dataset using Pandas.
3.  View the dataset structure, data types, and memory usage.
4.  Check for missing values and duplicates.
5.  Perform data summary statistics using `describe()`.
6.  View initial and final records using `head()` and `tail()`.
7.  Sort and group the data based on key attributes.
8.  Rename columns or modify data where necessary.
9.  Save the cleaned dataset for further analysis.

## Functions and Methods Used in the Experiment

### 1. Data Loading and Basic Exploration

  Function / Method      Description  
  ---------------------- ------------------------------------------------------\
  `pd.read_csv()`        Reads a CSV file into a Pandas DataFrame.\
  `df.info()`            Displays column names, data types, and memory usage.\
  `df.dtypes`            Returns the data types of all columns.\
  `df['column'].dtype`   Returns the data type of a specific column.\
  `df.describe()`        Provides summary statistics for numerical columns.\
  `df.head()`            Displays the first 5 rows of the DataFrame.\
  `df.tail()`            Displays the last 5 rows of the DataFrame.\

### 2. Sorting and Grouping

  Function / Method    Description  
  -------------------- ---------------------------------------------  
  `df.sort_values()`   Sorts the DataFrame by one or more columns.  
  `df.groupby()`       Groups data based on one or more columns.  
  `groupby.sum()`      Returns the sum for each group.  
  `groupby.mean()`     Returns the mean for each group.  
  `groupby.median()`   Returns the median for each group.  
  `groupby.max()`      Returns the maximum value in each group.  
  `groupby.min()`      Returns the minimum value in each group.  

### 3. Data Summary and Manipulation  

  Function / Method   Description  
  ------------------- ----------------------------------------------  
  `df.mean()`         Calculates the mean for all numeric columns.  
  `df.rename()`       Renames columns in the DataFrame.  

### 4. Optional Additions  

  Function / Method         Description  
  ------------------------- ----------------------------------------------  
  `df.isnull().sum()`       Checks for missing values in each column.  
  `df.duplicated().sum()`   Counts duplicate rows.  
  `df.dropna()`             Removes rows containing missing values.  
  `df.fillna()`             Fills missing values with a specified value.  
  `df.drop_duplicates()`    Removes duplicate rows.  
  `df.shape`                Returns the number of rows and columns.  
  `df.columns`              Lists column names in the DataFrame.  

## Program & Output
# Step 1: Import essential Python libraries  
import pandas as pd  
import numpy as np  
from pathlib import Path  
from IPython.display import display  

# Step 2: Load the dataset  
file_path = Path('heartdisease.csv')  

if not file_path.exists():  
    raise FileNotFoundError(  
        "heartdisease.csv was not found. Upload the file to the notebook folder "  
        "or update file_path with its correct location."  
    )

df = pd.read_csv(file_path)  
print('Dataset loaded successfully.')  
display(df.head())  

# Step 3: Explore dataset structure, data types, memory usage, and shape
print('Dataset information:')  
df.info()  

print('\nData types:')  
display(df.dtypes)  

print('\nDataset shape (rows, columns):', df.shape)  
print('\nColumn names:')  
print(df.columns.tolist())  
 
# Step 4: Check missing values and duplicate rows
print('Missing values in each column:')  
display(df.isnull().sum())  

print('Total missing values:', df.isnull().sum().sum())  
print('Number of duplicate rows:', df.duplicated().sum())  

# Step 5: Summary statistics
display(df.describe(include='all').T)  

# Step 6: View first and last five records
print('First five records:')  
display(df.head())  

print('Last five records:')  
display(df.tail())  

# Step 7: Sort the dataset by the first numeric column (if available)
numeric_columns = df.select_dtypes(include=np.number).columns.tolist()  

if numeric_columns:  
    sort_column = numeric_columns[0]  
    sorted_df = df.sort_values(by=sort_column)  
    print(f'Dataset sorted by: {sort_column}')  
    display(sorted_df.head())  
else:  
    print('No numeric column is available for sorting.')  

    # Step 8: Group data using the first categorical column, if available  
categorical_columns = df.select_dtypes(include=['object', 'category', 'bool']).columns.tolist()  

if categorical_columns:  
    group_column = categorical_columns[0]  
    print(f'Grouping by: {group_column}')  
    grouped = df.groupby(group_column, dropna=False)  
    print('\nGroup counts:')  
    display(grouped.size().rename('count').reset_index())  

    if numeric_columns:  
        print('\nGroup mean:')  
        display(grouped[numeric_columns].mean(numeric_only=True))  
        print('\nGroup median:')  
        display(grouped[numeric_columns].median(numeric_only=True))  
        print('\nGroup minimum:')  
        display(grouped[numeric_columns].min(numeric_only=True))  
        print('\nGroup maximum:')  
        display(grouped[numeric_columns].max(numeric_only=True))  
        print('\nGroup sum:')  
        display(grouped[numeric_columns].sum(numeric_only=True))  
else:  
    print('No categorical column is available for grouping.')  

    # Step 9: Data cleaning and manipulation
# Remove duplicate rows
cleaned_df = df.drop_duplicates().copy()  

# Fill missing numeric values with the column median.
# Fill missing categorical values with the column mode.
for column in cleaned_df.columns:  
    if cleaned_df[column].isnull().any():  
        if pd.api.types.is_numeric_dtype(cleaned_df[column]):  
            cleaned_df[column] = cleaned_df[column].fillna(cleaned_df[column].median())  
        else:  
            modes = cleaned_df[column].mode(dropna=True)  
            if not modes.empty:  
                cleaned_df[column] = cleaned_df[column].fillna(modes.iloc[0])  

print('Original shape:', df.shape)  
print('Shape after removing duplicates and handling missing values:', cleaned_df.shape)  
print('\nRemaining missing values:')  
display(cleaned_df.isnull().sum())  

# Optional: rename columns by editing the mapping below.
# Example: cleaned_df = cleaned_df.rename(columns={'old_name': 'new_name'})
display(cleaned_df.head())  

# Additional data summary: mean of numeric columns
print('Mean values of numeric columns:')  
display(cleaned_df.mean(numeric_only=True))  

# Step 10: Save the cleaned dataset
output_path = Path('heartdisease_cleaned.csv')  
cleaned_df.to_csv(output_path, index=False)  
print(f'Cleaned dataset saved as: {output_path.resolve()}')    




## Result

<< Enter the program execution result>>
