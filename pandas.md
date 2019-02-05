# Pandas

- `np.nan` === `NaN`

```Python
import pandas as pd
```


- PROP: `df.shape`: returns the dimensions of the df
- PROP: `df.columns`: returns the names of the columns as a Pandas Index
	- `df.columns = ['col1', 'col2', ...]` is also used to **assign** new column names
	- `df.index = ['A', 'B', 'C', ...]` can also be used to assign the indices
- `df.head(int x)`: returns the first `x` rows of the df
- `df.tail(int x)`: returns the last `x` rows of the df
- `df.info()`: Returns a summary of the DF and its columns (similar to `str` in R)
- Assign  the same value to a subset is possible by simple assignment:


```Python
df.iloc[5,:] = np.nan
```

**Note**: Indices have different types

	- `RangeIndex`: numbers

## Reading data

```Python

# From CSV
> df = pd.read_csv('file.csv', index_col = 0, header = [None|int], names = colnames : List, na_values = [String | [String] | Dict{'column_name': [String]}], parse_date = True)

# From Data frame
> dict = {'key': [a, b, c, d]}
> pd.DataFrame(dict)
```

## Creating new cols:

Use braodcasting.

```Python
> df['fees'] = 0
> df['sex'] = 'M'
```

## Subsetting data frames


#### Full columns

You can extract full columns by name:

```Python
col = df['column_name1'] # => pd.Series
```

To extract the numeric values: `col.values` (PROP) which returns a numpy array

#### `iloc`

Pandas uses the `iloc` function to subset by index. Apart from that the syntax is pretty much the same as R data frames or numpy 2D arrays:

```Python
AAPL.iloc[:5, :]
```


## Series

The columns of a data frame are a special type called `Series`


## Plots

Data frames and series have their own `.plot()` method that takes a ton of arguments.


```Python
df.plot(kind = 'scatter', x='colnameX', y='colnameY') # kinds: hist, 'box'
plt.title('Title of the plot')
plt.ylabel('Label of y')
plt.xlabel('Label of x')
plt.show() # <- Always call this func
```

Useful arguments to `plot`:

- `subplots = True`: generate a plot for each column independently

```Python
# This formats the plots such that they appear on separate rows

fig, axes = plt.subplots(nrows=2, ncols=1)
```



