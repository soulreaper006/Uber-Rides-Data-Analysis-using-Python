# Uber Dataset Analysis

## Overview

This project analyzes an Uber dataset, cleaning and transforming data while generating insights using visualizations.

## Features

- **Data Cleaning**: Handles missing values and duplicates.
- **Feature Engineering**: Extracts time-based features (day, night, month, etc.).
- **Data Visualization**: Uses seaborn and matplotlib to generate plots.
- **Statistical Analysis**: Computes correlations and distributions.

## Code

### Data Import

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

dataset = pd.read_csv("sourse/UberDataset.csv")
dataset.head()
```

### Data Inspection

```python
dataset.shape
dataset.info()
```

### Handling Missing Values

```python
dataset['PURPOSE'].fillna("NOT", inplace=True)
dataset.dropna(inplace=True)
dataset.drop_duplicates(inplace=True)
```

### Feature Engineering

```python
dataset['START_DATE'] = pd.to_datetime(dataset['START_DATE'], errors='coerce')
dataset['END_DATE'] = pd.to_datetime(dataset['END_DATE'], errors='coerce')
```

```python
from datetime import datetime

dataset['date'] = pd.DatetimeIndex(dataset['START_DATE']).date
dataset['time'] = pd.DatetimeIndex(dataset['START_DATE']).hour
dataset['day-night'] = pd.cut(x=dataset['time'], bins=[0,10,15,19,24], labels=['Morning','Afternoon','Evening','Night'])
```

### Exploratory Data Analysis

```python
plt.figure(figsize=(10,5))
sns.countplot(dataset['CATEGORY'])
plt.xticks(rotation=90)
```

```python
sns.countplot(dataset['day-night'])
plt.xticks(rotation=90)
```

```python
plt.figure(figsize=(15, 5))
sns.countplot(data=dataset, x='PURPOSE', hue='CATEGORY')
plt.xticks(rotation=90)
plt.show()
```

### Correlation Analysis

```python
numeric_dataset = dataset.select_dtypes(include=['number'])
sns.heatmap(numeric_dataset.corr(), cmap='BrBG', fmt='.2f', linewidths=2, annot=True)
```

### Time-Based Analysis

```python
dataset['MONTH'] = pd.DatetimeIndex(dataset['START_DATE']).month
month_label = {1: 'Jan', 2: 'Feb', 3: 'Mar', 4: 'April', 5: 'May', 6: 'June', 7: 'July', 8: 'Aug', 9: 'Sep', 10: 'Oct', 11: 'Nov', 12: 'Dec'}
dataset['MONTH'] = dataset.MONTH.map(month_label)
```

```python
dataset['DAY'] = dataset.START_DATE.dt.weekday
day_label = {0: 'Mon', 1: 'Tues', 2: 'Wed', 3: 'Thus', 4: 'Fri', 5: 'Sat', 6: 'Sun'}
dataset['DAY'] = dataset['DAY'].map(day_label)
```

```python
sns.boxplot(dataset['MILES'])
sns.boxplot(dataset[dataset['MILES']<100]['MILES'])
sns.distplot(dataset[dataset['MILES']<40]['MILES'])
```


