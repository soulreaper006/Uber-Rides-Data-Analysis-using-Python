# One-Hot Encoding with `sklearn.preprocessing.OneHotEncoder`

This script demonstrates how to perform one-hot encoding on categorical columns (`CATEGORY` and `PURPOSE`) using `OneHotEncoder` from `sklearn.preprocessing`. 

## Code

```python
import pandas as pd
from sklearn.preprocessing import OneHotEncoder

# Assuming dataset is already loaded and has the columns 'CATEGORY' and 'PURPOSE'
object_cols = ['CATEGORY', 'PURPOSE']

# Initialize the encoder
OH_encoder = OneHotEncoder(sparse=False, handle_unknown='ignore')

# Perform one hot encoding
OH_cols = pd.DataFrame(OH_encoder.fit_transform(dataset[object_cols]))

# Ensure the index matches the original dataset
OH_cols.index = dataset.index

# Get the feature names after transformation
OH_cols.columns = OH_encoder.get_feature_names_out(input_features=object_cols)

# Drop original columns from the dataset
df_final = dataset.drop(object_cols, axis=1)

# Concatenate the transformed one-hot encoded columns with the rest of the dataset
dataset = pd.concat([df_final, OH_cols], axis=1)
```

## Explanation
1. **Importing Necessary Libraries**: `pandas` is used for DataFrame manipulation, and `OneHotEncoder` is imported from `sklearn.preprocessing`.
2. **Selecting Categorical Columns**: The `object_cols` list contains the categorical column names to be encoded.
3. **Encoding the Data**:
   - `OneHotEncoder(sparse=False, handle_unknown='ignore')` is used to transform categorical variables.
   - The encoded values are stored in a new DataFrame `OH_cols`.
   - The original dataset index is assigned to `OH_cols` to maintain alignment.
   - Column names are updated using `get_feature_names_out()`.
4. **Merging Encoded Columns**:
   - The original categorical columns are dropped.
   - The transformed columns are concatenated with the dataset.

