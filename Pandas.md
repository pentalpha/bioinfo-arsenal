#What is it for?
Python library with a DataFrame class that's VERY useful for a lot of things.
Everything you can do with a Spreadsheet can be done better with Panda's DataFrame.

#Import
```py
import pandas as pd
```
You may also need to import numpy:
```py
import numpy as np
```
#Load CSV/TSV Files
```py
df = pd.read_csv(filePath, sep=separatorChar)
```
#Write the modified DataFrame
```py
df.to_csv(filePath, sep=separatorChar, index=writeUselessNewIndexesOnFileAndMakeAHugeMess)
```
#Utilities:
Some essential tricks for DataFrames

##Create new columns
```py
df['column'] = np.NaN
df['anotherColumn'] = ''
```
##Iterate row by row
```py
for index, row in df.iterrows():
    #do something
```
##Assign values to specific cell
```py
df.set_value(rowIndex, column, newValue)
```
##Call a lambda for every row (faster than a for loop)
```py
df.apply(lambda row: function(row), axis=1) #axis=1 for rows, no columns
```
Can be used to assign values to a whole column too:
```py
df['column'] = df.apply(lambda row: function(row), axis=1)
```
##Drop columns
```py
df.drop('columnX', axis=1, inplace=True)
```
##Drop rows which are not included in a set of indexes
```py
df_sliced = df.take(list(indexes_to_keep))
```