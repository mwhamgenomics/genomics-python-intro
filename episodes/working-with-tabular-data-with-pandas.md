---
title: Working with tabular data with Pandas
teaching: 90
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain the basic principle of tidy datasets
- Load a tabular dataset using base Pandas functions
- Determine the structure of a data frame including its dimensions and the datatypes of variables
- Subset/retrieve values from a data frame
- Understand how Pandas handles categorical data
- Discuss importing data from other file formats like Excel
- Save a data frame as a comma-delimited file

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I get started with tabular data and spreadsheets in Python?
- What are some best practices for reading data into Pandas?
- How do I save tabular data generated in Pandas?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Working with tabular data

A substantial amount of the data we work with in genomics will be tabular data,
this is data arranged in rows and columns - also known as spreadsheets. We could
write a whole lesson on how to work with spreadsheets effectively ([actually we did](https://datacarpentry.org/organization-genomics/)). For our
purposes, we want to remind you of a few principles before we work with our
first set of example data:

**1\) Keep raw data separate from analyzed data**

This is principle number one because if you can't tell which files are the
original raw data, you risk making some serious mistakes (e.g. drawing conclusions
from data which have been manipulated in some unknown way).

Tools like R and Pandas help with this because when you use them to work with
data, you are not changing the original file you loaded that data from. This is
different from spreadsheet programs where changing the value of a cell leaves you one
"save"-click away from overwriting the original file. In Pandas, you have to purposely
use a writing function to save data. You still need to choose a sensible filename, but
it's generally harder to accidentally overwrite your original file.

**2\) Keep spreadsheet data Tidy**

The simplest principle of **Tidy data** is that we have one row in our
spreadsheet for each observation or sample, and one column for every variable
that we measure or report on. As simple as this sounds, it's very easily
violated. Most data scientists agree that significant amounts of their time is
spent tidying data for analysis. Read more about data organization in
[our lesson](https://datacarpentry.org/organization-genomics/) and
in [this paper](https://www.jstatsoft.org/article/view/v059i10).

**3\) Verify your findings**

Finally, while you don't need to be paranoid about data, you should have a plan
for how you will prepare it for analysis. **This a focus of this lesson.**
You probably already have a lot of intuition, expectations, assumptions about
your data - the range of values you expect, how many values should have
been recorded, etc. Of course, as the data get larger our human ability to
keep track will start to fail (and yes, it can fail for small data sets too).
Pandas will help you to examine your data so that you can have greater confidence
in your analysis, and its reproducibility.


## Importing tabular data into Python

Unlike R, Python does not have dataframe-like functionality built in, however this capability is
possible via [Pandas](https://pandas.pydata.org), a third-party package built for this purpose.

::::::::::::::::::::::::::::::::::::::::  callout

## Installing Pandas

Pandas is highly popular, and as such is often included as standard in data science software solutions.
As such, if you are using Jupyter, chances are you already have Pandas installed. If you don't, it can
still be installed via the command line.

If you are using Python via a Conda environment or Python virtual environment, ensure that it is
activated. Then, on the command line:

```bash
pip install pandas
```

::::::::::::::::::::::::::::::::::::::::::::::::::

## Working with Python packages and modules

So far we have been using **built-in** functions, like `print()` and `type()`. Even if we have Pandas
installed, we can't use its functions yet, because we haven't loaded the package into memory.

Pandas has a function, [`read_csv()`](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html#pandas.read_csv),
that will load a CSV file. This function takes a single argument - the path to the CSV file to load.

There are two equivalent ways of making this function available and using it. We can load the entire
Pandas package and use **dot notation** to reference the function inside it:

```python
import pandas
pandas.read_csv('combined_tidy_vcf.csv')
```

Or we can load the function on its own. This method loads the function into the main namespace, meaning
that we don't need dot notation to reference it, but because we've only loaded that one function, we
can't use anything else from Pandas without further `import` statements.

```python
from pandas import read_csv
read_csv('combined_tidy_vcf.csv')
```


:::::::::::::::::::::::::::::::::::::::  challenge

## Reviewing the Pandas `read_csv()` function

Before using `read_csv()` further, let's read up on it and answer the following questions.

Run `help()` on the `read_csv()` function, or visit its documentation at https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html#pandas.read_csv.

A) What is the default parameter for 'header'?

B) What parameter would you have to change to read a file that was delimited
by semicolons (;) rather than commas?

C) What parameter would you have to change to read a file in which numbers
use commas for decimal separation (i.e. 1,00)?

D) What parameter would you have to change to read in only the first 10,000 rows
of a very large file?

:::::::::::::::  solution

## Solution

A) `header` is set to 'infer' by default, meaning that Pandas will try to
guess whether the first row is a header containing column names.

B) We can change the delimiter/separator by changing the `sep` argument,
which is set to `,` by default.

C) If you are reading a file containing differently-formatted decimals, you
can account for this by changing the `decimal` argument, which defaults to `.`.

D) You can set `nrows` to a numeric value to choose how many rows of a file
you read in. This may be useful for very large files where not all the data
is needed to test some data cleaning steps you are applying.

There are many more options available to `read_csv()` and other functions in
Pandas, and it's useful to be able to browse their documentation to find out
what they can do.

:::::::::::::::::::::::::::::::::::::::::  callout

## Positional and named arguments

Before this chapter, we have been using functions such as `print()` with **positional
arguments**:

```python
print(1, 2, 3)
```

Arguments are separated by commas if we have more than one, and are provided as an
ordered sequence.

The arguments we've just been looking at above are **named arguments**, where each one
is specified with a name (e.g. `header`, `nrows`, etc.), an `=` sign, and the value to
set the argument to.

Consider a situation where we want to read only the first 100 lines of a file
delimited by semicolons instead of commas:

```python
pandas.read_csv('some_semicolon_separated_file.csv', sep=';', nrows=100)
```

Here we have provided the input file as a **positional argument**, and the delimiter
and number of rows as **named arguments**. The order of named arguments does not matter,
so the following is equivalent:

```python
pandas.read_csv('some_semicolon_separated_file.csv', nrows=100, sep=';')
```
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::

Let's read in the file `combined_tidy_vcf.csv` and save it to a variable:

```python
# read in a CSV file and save it as 'variants'

variants = pandas.read_csv('combined_tidy_vcf.csv')

# or read it directly from the URL:
variants = pandas.read_csv('https://ndownloader.figshare.com/files/14632895')
```

If you `print()` this variable, you can see that it is a dataframe of 801 rows and 29 columns.

The majority of the columns in the data frame correspond to standard fields found in a 
*Variant Call Format (VCF)* file, while others were added during our data processing. The VCF 
format is a standard format for storing variant calls (also known as Single Nucleotide Polymorphisms or SNPs),
and you can read more about it, including a description of the fields we have here 
in [the VCF specification](https://samtools.github.io/hts-specs) 
or [on Wikipedia](https://en.wikipedia.org/wiki/Variant_Call_Format).

Suppose we have it saved to the variable `variants`, we can also see the dimensions of the
dataframe. Every Pandas dataframe has a value called `shape`:

```python
print(variants.shape)
# prints: (801, 29)
```

A couple of things are happening here:
- We reference `shape` on the dataframe with **dot notation**, the same as when
  we reference a function inside a package.
- We don't need brackets `()` here, because `shape` is not technically a function, it's just a data value.


## Selecting columns

We can see all the columns of a dataframe with `.columns`:

```python
print(variants.columns)
```

We can select a specific column out of a dataframe by **indexing** it with square brackets:

```python
print(variants['sample_id'])
```

We can also select multiple columns by providing a list to the indexing:

```python
subset = variants[['sample_id', 'CHROM', 'POS', 'ALT']]
```

(Note that indexing using a list works with Pandas dataframes, but not vanilla Python lists or strings.)

Every dataframe is a collection of columns, which in Pandas are represented as objects called [**Series**](https://pandas.pydata.org/docs/reference/series.html).

::::::::::::::::::::::::::::::::::::::::  callout

## Selecting data by row/column numbers

Dataframes also have [`iloc`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.iloc.html)
for selecting data by labels, and column and row numbers, and
[`loc`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.loc.html) for selecting by column
and row numbers. Both of these features are useful, however they are beyond the scope of this course.

::::::::::::::::::::::::::::::::::::::::::::::::::


## Summarizing and determining the structure of a data frame

A data frame stores **tabular data**. It can be thought of as a collection of vectors/lists,
all of which have the same length. There are two ways we can learn more about our data. First, let's
check out some summary statistics:

```python
variants.describe()
```

Again note the dot notation, and the brackets indicating that this is a function call. Functions
that live inside an object like this are also known as **methods**.

If we run this, we get summary information for the numerical columns in the dataframe. For columns like
`QUAL`, `IMF`, and `VDB` this works well, but it's not perfect. The `gt_GT` column is not
strictly a numerical column yet it has been treated like one, and text columns are not included.
Nonetheless, it's useful for a quick look at your numerical data.

Next, let's use `dtypes` to see the data type of each column:

```python
print(subset.dtypes)
```

`int64` and `float64` are data types that are not built into Python as standard, but are part of Pandas.


## Categorical data

R has factors for dealing with non-continuous data types, such as genders, blood types and nationalities,
and Pandas is able to work in a similar way on columns with a string type.

Let's explore the value of this by taking a look at just the ALT column of our dataframe. We can select
a column by **indexing** the the dataframe with square brackets and the string name of the column:

```python
alt_alleles = subset['ALT']
```

Let's look at the first few items in our factor using `head()`:

```python
alt_alleles.head()
```

We can see we have a mix of single-nucleotide variants and multi-nucleotide variants.


### Selecting data by value

There are 801 alleles, one for each row. To simplify things, lets look at just the
single-nucleotide alleles (SNPs). To do this, we need to select just the rows where ALT is a
single A, T, G or C.

To do this, we can use logical comparators as seen in this course's introduction to Python
conditionals:

```python
alt_alleles == 'T'
```

```output
0      False
1      True
2      True
3      False
4      False
       ...  
796    False
797    False
798    False
799    False
800     True
Name: ALT, dtype: bool
```

This may not look very useful, but what we have created is a **mask** - a series of boolean values
depending on a condition - if the value is 'T', we get True, otherwise we get False. Let's introduce
a more complex condition, checking for all four DNA bases:

```python
snp_mask = (alt_alleles == 'A') | (alt_alleles == 'T') | (alt_alleles == 'G') | (alt_alleles == 'C')
print(snp_mask)
```

```output
0       True
1       True
2       True
3      False
4      False
       ...  
796     True
797    False
798    False
799     True
800     True
Name: ALT, Length: 801, dtype: bool
```

We get a mask that is True where the ALT column is a single A, T, G or C, otherwise False.

We're chaining together four conditions similar to the introduction to conditionals, however Pandas
needs this to be done with pipes (`|`) and ampersands (`&`) rather than `or` and `and`. As a brief
summary of why this is, `|` and `&` are **bitwise operators** that are suited to arrays of values.
For more detailed information, you can read about [bitwise operations](https://en.wikipedia.org/wiki/Bitwise_operation#OR).

We also enclose each condition inside brackets to prevent Pandas from getting too confused when
parsing the chain of conditions.

Now we have a mask, we can use this to index the dataframe:

```python
snps = variants[snp_mask]
print(snps)
```

We now have a new dataframe of all single-nucleotide variants in the dataset.


## Saving your data frame to a file

We can save dataframes to a file using the [`to_csv()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html)
method. This requires a single argument, the filename to save it as:


```python
snps.to_csv('snps.csv')
```

Looking at the resulting file, we see an unexpected column has been added:

```
,sample_id,CHROM,POS,ID,REF,ALT,...
0,SRR2584863,CP000819.1,9972,,T,G,...
1,SRR2584863,CP000819.1,263235,,G,T,...
```

This is the **index** of the dataframe, which by default is just row numbers, and is included in
the output of `to_csv()`. We can remove this by adding the argument `index=False`:

```python
snps.to_csv('snps.csv', index=False)
```

::::::::::::::::::::::::::::::::::::::::  callout

## Importing data from Excel

Excel is a common file format, so we need a way of working nicely with these files in Pandas. The
simplest way would be to save the Excel file in .csv format so that we can use `pandas.read_csv()`
as before, however this may not always be possible (suppose you have data in 300 Excel files,
opening and exporting all of them is not practical).

As a result, in reality, we'll inevitably at some point need to work directly with Excel spreadsheets.
Fortunately, Pandas is able to do this:

```python
pandas.read_excel('data.xlsx')
```

This works in the same way as `pandas.read_csv()` - it still produces a dataframe, and even takes a lot
of the same arguments. Having consistent, familiar interfaces like this is one of the things that makes
data science packages like Pandas and R so versatile and powerful.

Note that if you are using a self-installed Python instance, you may need to install an Excel engine
separately, such as [openpyxl](https://pypi.org/project/openpyxl). Once Pandas has an engine available,
it should use it automatically.

:::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::  challenge

## Putting it all together - data frames

Use `read_excel()` to read the file 'Ecoli_metadata.xlsx' into a dataframe, and answer the following questions:

A) What are the dimensions (# rows, # columns) of the data frame?

B) What are all the distinct values of the `cit` column? (Hint: Once you have a column selected, you can call a method on it named [`unique()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.unique.html))

C) How many of each value are there in the `cit` column? (Hint: look in the [Series documentation](https://pandas.pydata.org/docs/reference/series.html) for a method that will **count** the **values** of the column)

D) What is the genome size for the 7th observation in this data set? (Hint: Pandas Series objects can be indexed like lists)

:::::::::::::::  solution

## Solution

```python
meta = pandas.read_excel('Ecoli_metadata.xlsx')

# A)
print(meta.shape)

# B)
print(meta['cit'].unique())

# C)
print(meta['cit'].value_counts())

# D)
print(meta['genome_size'][6])
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Pandas can work with data saved in many formats, including CSV and Excel
- A dataframe is a collection of columns, which Pandas calls Series
- There are many ways of selecting data from dataframes, including indexing, column selection, and masking

::::::::::::::::::::::::::::::::::::::::::::::::::