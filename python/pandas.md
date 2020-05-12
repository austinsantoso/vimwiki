[Home](../index.md)

# Pandas
Make sure Pandas in Installed

## Getting started
```python
import pandas as pd
```

## Creating Data
### Dataframe
A `Dataframe` is a table. It contains an array of individual entries, each of which has a certain value.
Each entry is corresponds to a row(or record) and column

To create a simple data frame.
Using a dictionary of arrays:
```python
pd.DataFrame({'Yes':[50,10], 'No': [131, 2]}, index=['Product A', 'Product B'], columns=['Yes', 'No'])
```
This would create the following table.
|           | Yes | No  |
|-----------|-----|-----|
| Product A | 50  | 131 |
| Product B | 21  | 2   |

The list of row tables used in a DataFrame is known as an *Index*.
We can set it using the `index` parameter.
We can also select the columns to use by their names by using `columns` parameter.
It will select the columns based on the name in the `columns` parameter.
By default it will show all columns based on their order in the dictionary.

### Series
A `Series` is just a special dictionary.
The index is the dictionary key, the value is the value.
We can access an index using the key name.

To create one:
```python
pd.Series([1,2,3,4,5], index=['a', 'b', 'c', 'd', 'e'], name='some name')
```
This will create the following
| a | 1 |
| b | 2 |
| c | 3 |
| d | 4 |
| e | 5 |
`Name: some name, dtype:int64`

We can indicate the index to use using `index`.
A `Series` Does not have any column names, it only has one over all name, as indicated by the `name` parameter.

## Reading and Saving Data
We can read data from a `.csv`:
```python
df = pd.read_csv('path/to/csv')
```

To write to a `.csv` file:
```python
df.to_csv('output_file.csv')
```

Optionally, we can let pandas choose a column to be the index column using the `index_col` parameter.

## Viewing Data

To get an idea of the size of the dataframe, getting the `(row, col)`:
```python
df.shape
```

To grab the first 5 rows:
```python
df.head()
```

## Accessing Data
To view the entire table
```python
df
```

To view only one column, This would get us a specific `Series` from the Dataframe.
```python
df.columnName
df[columnName]
```

## Indexing in Pandas
To access data in the DataFrame, based on indexes or attributes.

Both `iloc` and `loc` are row-first, column-second. `[rows, cols]`

### iloc
Select data based on its *numerical* position in the data.

To select first row
```python
df.iloc[0]
```

To get the all rows, first columng
```python
df.iloc[:, 0]
```

### loc
Label based selection. We use the data index value, rather than the position.

To access the first row, and _country_ column.
```python
df.loc[0, 'country']
```

To access the all rows, and some columns.
```python
df.loc[:, ['col1', 'col3']]
```

### set index
We can set the index of the dataframe to dataframe using:
```python
df.set_index("column_name")
```

## Conditional Selection
To select things based on conditions.

We can perform
```python
df.country == 'Italy'
```
This operation produced a Series of `True`/`False` booleans based on the country of each record.
This result can then be used inside of loc to select the relevant data:
```pyton
df.loc[review.country == 'Italy']
```
This will filter out and only select those with country _Italy_.
We can use the `&` (ampersand) to combine two conditions,
or use `|` (pipe) to indicate or condition.

Pandas also comes with some useful functions to help filter data.

`isin` selects data whose values "is in" a list of values
```python
df.loc[df.country.isin(["Italy", "France"])]
```

`isnull` and its friend `notnull` selects values which are or not empty (NaN).
To remove the nulls/NaN in price column:
```python
df.loc[review.price.notnull()]
```

# Assigning data
We can assign a constant value, this will set the same value for all:
```python
df['critic'] = 'everyone'
```
or an iterable of values:
```python
df['reverse_index'] = range(len(reviews), 0, -1)
```

## Summary Functions
These functions are created to give a summary of the data.

To generate a high-level summary of the atributes of the given column.
This can be used to describe any kind of data: numerical or text.
```python
df.points.describe()
```

For functions like mean and median, there are built-in functions. To get the mean:
```python
df.points.mean()
```


To get a list of unique variables in a column:
```python
df.country.unique()
```

To see a list of unique values and how often they occur in the dataset:
```python
df.country.value_counts()
```

Some other useful functions:
* `idxmax` to get the index of max value
*

## Map
Apply functions to all variables.
Note that `map()` and `apply()` return new, transformed Series and DataFrames, respectively.

### map
The function you pass to `map()` should expect a single value from the Series (a point value, in the above example),
and return a transformed version of that value.
`map()` returns a new Series where all the values have been transformed
by your function.
```python
df.points.map(lambda p: p - points_mean)
```
### apply
apply  is the equivalent method if we want to transform a whole DataFrame by calling a custom method on each row.
```python
def remean_points(row):
    row.points = row.points - review_points_mean
    return row

reviews.apply(remean_points, axis='columns')
```
If we had called reviews.apply() with axis='index',
then instead of passing a function to transform each row,
we would need to give a function to transform each column.

### built-in mappings
Pandas provides many common mapping operations as built-ins. For example, here's a faster way of remeaning our points column:
```python
review_points_mean = reviews.points.mean()
reviews.points - review_points_mean
```
This would subtract each value in `reviews.points` by `review_points_mean`

