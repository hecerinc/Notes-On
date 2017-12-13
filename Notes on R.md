# Notes on R

## Important things

1. Vectors in R are **1 - indexed**.
2. Use the **`class()`** function to find out what type of object you're dealing with.
3. **`ncol()`**: get # of cols
4. **`nrow()`**: get # of rows
5. Typing `?function_name` in R will open the documentation for that function in your default browser
6. The **`str()`** function displays the internal structure of an R object and a function's declaration.
7. `head()` will print the first rows of a data frame.
8. R **stores datasets in RAM memory.** You cannnot load data that is larger than your RAM.

## Input

### `<-` 

The `<-` left arrow operator is the **assignment** operator in R.

```R
x <- 1
print(x)
[1] 1
```

### Comments

Comments in R are prepended by hashes. No multiline comments exist in R.

```R
# Hello this is a comment
```

### Printing & auto-printing

In the R prompt, an expression will get auto-evaluated and if it returns something, it will be auto-printed. This will not work for R scripts. 
When an R vector is printed, you will see that an index for the vector is printed on the left of each line the output takes. These numbers are not part of the vector itself but are indices of the first element of that particular line. 

```R
> x <- 11:30
> x
  [1] 11 12 13 14 15 16 17 18 19 20 21 22
 [13] 23 24 25 26 27 28 29 30
```

### Ranges

Ranges in R are done via the `:` operator:

```R
> x <- 11:30
> x
  [1] 11 12 13 14 15 16 17 18 19 20 21 22
 [13] 23 24 25 26 27 28 29 30
```

## Objects

Everything in R is an object. Most things in R work based on **vectors**, since it is the most basic type. The whole language has 6 types of atomic objects:

- character
- numeric (real numbers) (`"double"` exists as synonym for this type)
- integer
- complex 
- logical (True / False)
- raw

### Vectors

The only rule for R vectors is that they have to contain **objects of the same type**.

#### `vector(mode, length)`

https://www.rdocumentation.org/packages/base/versions/3.3.2/topics/vector

The vector function is used to create a vector of a specified `mode` and `length`.

- **`mode`**:  One of the 6 atomic types above **OR** `"list"` __OR__ `"expression"` __OR__ `"any"`.
- **`length`**: a non-negative integer specifying the desired length

**Return value**: a vector of the desired `mode` and `length`. Logical vectors are initialized to `FALSE`, numeric elements to `0`, character vector elements to `""`, raw vector elements to `nul` bytes and list/expression elements to `NULL`. 

### Numbers

- All numbers in R are treated as `double` precision real numbers. 
- `Inf` is a special number representing infinity. 
- If you want an integer, append an `L` to the number. 
- `NaN` stands for Not a Number

### Attributes

R objects can have attributes, which are like metadata for the object. Column names in a dataframe, dimensions, class, length and other user-defined attributes are all examples of attributes. These attributes can be accessed via the **`attributes()`** function.
	
### `c()`

The `c()` function can be used to create vectors by concatenating objects.

```R
> x <- c(0.5, 0.6) # numerical
> x <- c(TRUE, FALSE) # logical
> x <- c(T, F) # logical
> x <- c("a", "b", "c") # character
```

#### Coercion

If you mix more than one object type in a vector, R will do implicit coercion to try to represent all the objects in the same representation. However, you can also **explicitly** coerce values using the `as.*` functions.

```R
> x <- 0:6
> class(x)
[1] "integer"
> as.numeric(x)
[1] 0 1 2 3 4 5 6
> as.logical(x)
[1] FALSE TRUE TRUE TRUE TRUE TRUE TRUE
> as.character(x)
[1] "0" "1" "2" "3" "4" "5" "6"
```

### Matrices

Matrices are vectors with a _dimension_ attribute, which is itself a vector of length 2 (row and col). All the elements **must** have the same class.

```R
> m <- matrix(nrow = 2, ncol = 3)
> m
     [,1] [,2] [,3]
[1,]   NA   NA   NA
[2,]   NA   NA   NA
> dim(m)
[1] 2 3
> attributes(m)
$dim
[1] 2 3
```

Matrices are constructed column-wise. Fills columns first, then rows.

**Example:**

```R
> m <- matrix(1:6, ncol=3, nrow=2)
> m
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
```

They can also be created from vectors by adding a `dimension` attribute to the vector.

```R
> m <- 1:10
> m
 [1]  1  2  3  4  5  6  7  8  9 10
> dim(m) <- c(2,5)
> m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    3    5    7    9
[2,]    2    4    6    8   10
```

#### `cbind()` & `rbind()`

Matrices can also be created by _column-binding_ and _row-binding_.

```R
> x <- 1:3
> y <- 10:12
> cbind(x, y)
     x  y
[1,] 1 10
[2,] 2 11
[3,] 3 12
> rbind(x, y)
  [,1] [,2] [,3]
x    1    2    3
y   10   11   12
```

### Factors

Factors are a special type of object that are used to represent categorical data that can be unordred or ordered. One can think of a factor as an integer vector that has _labels_. Having a factor is better than an integer vector because factors are self-describing. Having a value that has "Male" and "Female" is much more useful than one that has 1 and 2s.

```R
> x <- factor(c("yes", "yes", "no", "yes", "no"))
> x
[1] yes yes no  yes no
Levels: no yes
> table(x)
x
 no yes
  2   3
> unclass(x) # underlying representation
[1] 2 2 1 2 1
attr(,"levels")
[1] "no"  "yes"
```

### Lists

Lists are a special type of vector that can contain different types of objects.

#### `list()`

```R
> x <- list(1, "a", TRUE, 1 + 4i)
```

## Data Frames

Data frames are used to store tabular data in R. They are internally stored as a list of lists. Data frames are not restricted to a type of object.
Data frames have **both** column names **and** row names, which indicate information of each row in the data frame. Data frames are usually read in from a file but can also be created explicitly or coerced from lists.

```R
> x <- data.frame(foo=1:4, bar=c(T, T, F, F))
> x
  foo   bar
1   1  TRUE
2   2  TRUE
3   3 FALSE
4   4 FALSE
> nrow(x)
[1] 4
> ncol(x)
[1] 2
> attributes(x)
$names
[1] "foo" "bar"

$row.names
[1] 1 2 3 4

$class
[1] "data.frame"
```

### Names

Almost all vectors and compound data structures can be given names in R with one of the following functions:


| Object | Set column names | Set row names |
| ------ | ---------------- | ------------- |
| data frame | `names()` | `row.names()` |
| matrix | `colnames()` | `rownames()` | 

`dimnames()`

```R
> m <- matrix(1:4, nrow = 2, ncol =2)
> m
     [,1] [,2]
[1,]    1    3
[2,]    2    4
> dimnames(m) <- list(c("a", "b"), c("c", "d"))
> m
  c d
a 1 3
b 2 4
```

`colnames()` && `rownames()`

```R
> colnames(m) <- c("h", "f")
> rownames(m) <- c("x", "z")
> m
  h f
x 1 3
z 2 4
```

`names()`: 

```R
> x <- 1:3
> names(x) <- c("New York", "Boston", "Seattle")
```

## Reading data in & out

Main functions for **reading** data:

- `read.table`, `read.csv` for reading tabular data
- `readLines`, for reading lines of a text file
- `source`, for reading in R code files (inverse of `dump`)
- `dget`, for reading in R code files (inverse of `dput`)
- `load`, for reading in saved workspaces
- `unserialize`, for reading single R objects in binary form


Analogous functions for **writing** data:

- `write.table`, for writing tabular data to text files (i.e. CSV) or connections
- `writeLines`, for writing character data line by line to a file or connection
- `dump`, for dumping a textual representation of multiple R objects
- `dput`, for outputting a textual representation of an R object
- `save`, for saving an arbitrary number of R objects in binary format to a file
- `serialize`, for converting an R object into a binary format for outputting to a connection (or file)

## Reading large datasets

#### `read.table`

When reading a large datasets you can do some preprocessing work to make the process more efficient:

- Make a rough calculation of the memory required to store the dataset. **R stores the datasets in RAM** so if size(dataset) >= size(RAM) stop right there.
- Set `comment.char = ""` if there are no commented lines in your file.
- Use the `colClasses` arguments to specify what data type your columns hold. R will try to guess this if you don't, but the process is **much faster** when you do it yourself.
- Set the `nrows` argument. This does not make the process faster but it helps with memory usage.

## Subsetting R objects

There are 3 operators to subset objectsi n R:

- **`[]` operator**:  always returns an object of the same class. It can be used to select *multiple* elements of an object
- **`[[]]` operator**: is used to extract a **single** element of a list or data frame. 
- **`$` operator**: is used to extract elements of a list or data frame by literal name. 

### Vectors

```R
> x <- c("a", "b", "c", "c", "d", "a")
> x[1]  # Extract the first element
[1] "a"
> x[2]  # Extract the second element
[1] "b"
> x[1:4]
[1] "a" "b" "c" "c"
> x[c(1, 3, 4)]
[1] "a" "c" "c"

# You can also pass logical sequences:

> x[x > "a"] # x > "a" creates a logical vector, but we don't need to store that
[1] "b" "c" "c" "d"

```

### Matrices

When subsetting matrices, R will automatically drop the dimensions (i.e. it will return a vector of length n instead of a matrix with dimensions `1xN`). **Be careful.**

```R
> x <- matrix(1:6, 2, 3)
> x
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

> x[1,2] # get element (1, 2)
[1] 3
> x[2,1] # get element (2, 1)
[1] 2

> x[1,] # get the first row
[1] 1 3 5

> x[,2] # get the second column
[1] 3 4
```

### Lists

```R
> x <- list(foo= 1:4, bar = 0.6)
> x
$foo
[1] 1 2 3 4

$bar
[1] 0.6

## Extract a single element [[]]

> x[[1]]
[1] 1 2 3 4 
> x[["bar"]]
[1] 0.6
> x$bar
[1] 0.6

## Nested elements

> x[[c(1,3)]]
> x[[1]][[3]] # same as above
```

### Removing `NA` values

```R
> x <- c(1, 2, NA, 3, NA, 5)
> x
[1]  1  2 NA  3 NA  5
> bad <- is.na(x)
> bad
[1] FALSE FALSE  TRUE FALSE  TRUE FALSE
> x[!bad]
[1] 1 2 3 5
```

#### `complete.cases`

`complete.cases` is a function that given a set of vectors or columns of a data frame, will return a **logical** vector with the rows that are not `NA` for **all of the columns**.


## Vectorized Operations

Operations in R are vectorized.

Examples:

```R
# Vector summation (substraction, multiplication, division)

> x <- 1:4
> y <- 6:9
> x + y
[1]  7  9 11 13


# Selecting elements

> x > 2
[1] FALSE FALSE  TRUE  TRUE
> x[x > 2]
[1] 3 4


```

Notice how `x > 2` returns a boolean vector. You can then use this vector to subset the vector and select elements that match that condition.

### Matrix Operations

This principle also applies to matrix operations such as: 

- matrix (element-wise) multiplication, division

**Matrix multiplication**:

```R
> rep(10,4)
[1] 10 10 10 10
> x <- matrix(1:4, 2, 2)
> y <- matrix(rep(10, 4), 2, 2)
> y
     [,1] [,2]
[1,]   10   10
[2,]   10   10
> x
     [,1] [,2]
[1,]    1    3
[2,]    2    4
> x %*% y
     [,1] [,2]
[1,]   40   40
[2,]   60   60
> x * y
     [,1] [,2]
[1,]   10   30
[2,]   20   40
```


