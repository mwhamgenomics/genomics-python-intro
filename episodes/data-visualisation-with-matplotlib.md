---
title: Data Visualization with matplotlib
teaching: 90
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Plot numerical data as a scatter plot
- Customize plot scales, titles and colours
- Plot multiple samples together in one graph
- Make a figure of multiple plots side by side
- Save a plot to a file.
- Find out where to get help with matplotlib
- Use loops and functions to make our plotting code more efficient

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What is matplotlib?
- How can I make publication-quality plots in Python?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction to matplotlib

<img src="https://matplotlib.org/stable/_static/logo_light.svg" align="right" alt="Matplotlib logo">

**`Matplotlib`** is a plotting package for Python with extensive integration with Pandas for creating
complex plots from dataframes. It programmatic interface lets the user specify what variables to plot,
how they are displayed, and general visual properties like colours. Therefore, we can update a plot with
minimal code changes, e.g. if the underlying data changes or if we decide to change from a bar plot to a
scatter plot. This helps in creating publication-quality plots with minimal amounts of adjustments and tweaking.

## Installing matplotlib

Like Pandas, matplotlib is likely to be already included in standard data science packages like Jupyter. If not though,
it can be installed via pip:

```bash
pip install matplotlib
```

Then to load it in Python:

```python
import matplotlib.pyplot as plt
```

- We use dot notation here to drill down into matplotlib and fetch only the bit of it that we want
- We use `as` to assign a shorter name for the imported package. If we didn't add this bit at the end, we'd have to type out `matplotlib.pyplot.function_name()` every time.

## Loading the dataset

You'll still need Pandas to load the data, so import it and load the file if not already done so:

```python
import pandas
variants = pandas.read_csv('combined_tidy_vcf.csv')
```

Take a quick look at the dataset. We've already seen `shape` and `columns` from the last chapter, but dataframes also have useful
`head()` and `tail()` methods to show only the first/last few rows:

```python
variants.head()
variants.tail()
```

As a reminder, data is easier to work with and plot when it is **tidy**, also known as **long format**:

```
site           year  cases
Whitehorse     1999  745
Whitehorse     2000  2666
Yellowknife    1999  37737
Yellowknife    2000  80488
Inuvik         1999  212258
Inuvik         2000  213766
```

As opposed to **wide format**:

```
site         1999    2000
Whitehorst   745     2666
Yellowknife  37737   80488
Inuvik       212258  213766
```

## Plotting

Like [ggplot2](https://ggplot2.tidyverse.org), plots are built up step by step by adding elements. This lets us customise
plots very flexibly.

Let's start by making a scatter plot of chromosomal position against depth of coverage. Matplotlib has a `scatter()` function
that takes a minimum of two arguments, corresponding to the x and y axes respectively.

```python
plt.scatter(variants['POS'], variants['DP'])
```

If running Python on the command line outside of Jupyter, you may need to also run `plt.show()` to get the plot to actually display.

It looks a bit blotchy at the moment - let's fix that.

```python
plt.scatter(variants['POS'], variants['DP'], s=12, alpha=0.5, edgecolors='none')
```

Here we use the arguments `s` for point size, `alpha` for transparency (so we can see points layered up on top of each other),
and `edgecolors` to make them solid dots rather than bordered.

What if we want to be able to see each sample as a different colour? In ggplot2, this can be done by passing the `sample_id`
column as a dimension, but it's done differently in Pandas. First, let's see what samples we have:

```python
print(variants['sample_id'].unique())
```

We can see that we have 3 samples. Next, we need to split up the dataframe by each one using **masks**:

```python
srr2584863 = variants[variants['sample_id'] == 'SRR2584863']
srr2584866 = variants[variants['sample_id'] == 'SRR2584866']
srr2589044 = variants[variants['sample_id'] == 'SRR2589044']
```

Now if we pick three standard matplotlib colour names from the [docs](https://matplotlib.org/stable/users/explain/colors/colors.html),
we can plot each series with a different colour:

```python
plt.scatter(srr2584863['POS'], srr2584863['DP'], c='blue', s=12, alpha=0.5, edgecolors='none')
plt.scatter(srr2584866['POS'], srr2584866['DP'], c='gold', s=12, alpha=0.5, edgecolors='none')
plt.scatter(srr2589044['POS'], srr2589044['DP'], c='red',  s=12, alpha=0.5, edgecolors='none')
```

It's not a finished plot until it's labelled properly. We can show a colour legend, X/Y axis labels
and a main plot title with `legend()`, `xlabel()`, `ylabel()` and `title()`:

```python
plt.scatter(SRR2584863['POS'], SRR2584863['DP'], c='blue', s=12, alpha=0.5, edgecolors='none', label='SRR2584863')
plt.scatter(SRR2584866['POS'], SRR2584866['DP'], c='gold', s=12, alpha=0.5, edgecolors='none', label='SRR2584866')
plt.scatter(SRR2589044['POS'], SRR2589044['DP'], c='red',  s=12, alpha=0.5, edgecolors='none', label='SRR2589044')
plt.legend()
plt.xlabel('Position')
plt.ylabel('Coverage')
plt.title('Read depth vs. position')
```

Finally, if we want to save the plot as a file:

```python
plt.savefig('read_depth_vs_position.png')
```

Now the figure is complete and ready to be exported and saved to a file. This can be done with
[`savefig()`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html), which
will save the most recently generated figure as an image file. The format it will be saved as is
determined automatically from the filename specified - matplotlib can save jpeg, png, svg and pdf
files. If we check the current working directory, there should be a newly created file containing
your plot.

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge

Use what you just learned to create a scatter plot of mapping quality (`MQ`) over
position (`POS`) with the samples showing in different colors. Make sure to give your plot
relevant axis labels.

:::::::::::::::  solution

## Solution

```python
plt.scatter(srr2584863['POS'], srr2584863['MQ'], c='blue', s=12, alpha=0.5, edgecolors='none', label='SRR2584863')
plt.scatter(srr2584866['POS'], srr2584866['MQ'], c='blue', s=12, alpha=0.5, edgecolors='none', label='SRR2584866')
plt.scatter(srr2589044['POS'], srr2589044['MQ'], c='red',  s=12, alpha=0.5, edgecolors='none', label='SRR2589044')
plt.legend()
plt.xlabel('Position')
plt.ylabel('Coverage')
plt.legend()
plt.title('Mapping quality vs. position')
```

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::

## Histograms

Matplotlib has [far more plot styles](https://matplotlib.org/stable/plot_types/index.html),
than just scatter plots. Suppose we want to plot a histogram of coverage per variant:

```python
plt.hist(variants['DP'], bins=50)
```

The `bins` argument tells matplotlib how many bins to split the data into. From this,
we can see that the most read depth per variant is about 10 reads.


## Subplots

Sometimes, we want to make a panel of multiple plots. We can do this using **subplots**. Let's
split up our triplex scatter plot into three, one for each sample.

```python
fig = plt.figure(figsize=(12, 4))

plt.subplot(1, 3, 1)
srr2584863 = variants[variants['sample_id'] == 'SRR2584863']
plt.scatter(srr2584863['POS'], srr2584863['MQ'], s=12, alpha=0.5, edgecolors='none')
plt.title('SRR2584863')

plt.subplot(1, 3, 2)
srr2584866 = variants[variants['sample_id'] == 'SRR2584866']
plt.scatter(srr2584866['POS'], srr2584866['MQ'], s=12, alpha=0.5, edgecolors='none')
plt.title('SRR2584866')

plt.subplot(1, 3, 3)
srr2589044 = variants[variants['sample_id'] == 'SRR2589044']
plt.scatter(srr2589044['POS'], srr2589044['MQ'], s=12, alpha=0.5, edgecolors='none')
plt.title('SRR2589044')

fig.suptitle('Mapping quality vs. position')
fig.supxlabel('Position')
fig.supylabel('Mapping quality')
```

First we create a **figure** object, to contain everything and control how big it will be on the page. The
first number is the width and the second number is the height.

Then, for each sample, we:

- Call `subplot()` with a row and column configuration. When we call `subplot(1, 3, 2)`, we are
  telling it to work within **1** row, **3** columns, and to make a plot at position **2** (confusingly, counting
  from 1 unlike vanilla Python).
- Select the data that we want to plot as before, and save it to a variable (it's included here both as a reminder,
  and because it's about to become important in the next exercise)
- Call `scatter()` on the selected data as before
- Add a title

Since we have a figure of three plots, we also have the option of having one set of axis labels for the whole figure
rather than three identical ones. The figure object has a set of methods, `supxlabel()`, `supylabel()` and `suptitle()`
that can be called in the same way as the top-level pyplot equivalents.

:::::::::::::::::::::::::::::::::::::::  challenge

## Repetitive code

The code for creating these plots per sample looks quite repetitive. What are some of the potential results of this?

:::::::::::::::  solution

## Solution

- It's a lot of code to write
- It's a lot of code for other people to read and understand
- Sample IDs are 'hard-coded', so if you load a different dataset, this code won't be able to plot it

:::::::::::::::::::::::::

## Loops

We can use **loops** reduce code duplication. Using the guidance in the first chapter, write a loop
that will plot all three samples.

A few notes:

- You can list all the samples in the dataset with `variants['sample_id'].unique()`
- Some tasks only need to be done once, while others will need to be done for each sample. As such, some
  expressions will be inside the loop, and others will not.
- Think about each step described above for making each plot.
- You'll need to **increment** the plot number each time in the call to `subplot()`.

### Bonus:

Python has a built-in function called `len()`, that will return the length of a list, string, etc. Use this to
make the loop able to take a dataset with any number of samples.

:::::::::::::::  solution

## Solution

```python
fig = plt.figure(figsize=(12, 4))
plot_number = 1
samples = variants['sample_id'].unique()
total_plots = len(samples)

for sample in samples:
   plt.subplot(1, total_plots, plot_number)
   sample_data = variants[variants['sample_id'] == sample]
   plt.scatter(sample_data['POS'], sample_data['MQ'], s=12, alpha=0.5, edgecolors='none')
   plt.title(sample)

   plot_number = plot_number + 1

fig.suptitle('Mapping quality vs. position')
fig.supxlabel('Position')
fig.supylabel('Mapping quality')
```

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::


## Functions

We've been using many functions throughout this course, and this last part is on writing our own. Functions
are a powerful tool for encapsulating bits of code that we write into reusable chunks. Using them, we can
reduce code duplication and complexity, make fault-finding easier, and make the scripts we write more versatile
and scalable.

We create a function using the keyword `def`; the name of the function; the arguments that we want it to have
inside brackets (even if the number of arguments is 0), and a colon to end the line:

```python
def print_something():
   print('Hello world')
```

Here we define a function with no arguments, that will print 'Hello world':

```python
print_something()
```

Let's make the function more flexible. We do this by choosing a name for the argument and adding it inside
the brackets of the function declaration. We can then use the argument like a variable:

```python
def print_something(thing_to_print):
   print(thing_to_print)
```

The function now needs to called with an argument:

```python
print_something('Something to print')
```

We can make it even more flexible by adding a **default** parameter using `=` and the value we want the
argument to default to:

```python
def print_something(thing_to_print='Hello world'):
   print(thing_to_print)
```

Now the function can be called with one argument or with no argument, in which case it will fall back on
the argument's default.

Let's look at a more complex function:

```python
def print_something(thing_to_print, number_of_times=1, upper_case=False):
   for i in range(number_of_times):
      if upper_case == True:
         print(thing_to_print.upper())
      else:
         print(thing_to_print)
```

Here we see function definitions, for loops and if statements all working together. We also see
a couple of new functions/methods:

- `range()` - produces a sequence of numbers. Useful if you want a loop to happen a set number of times.
- `upper()` - make a string upper-case

We can call this function in many different ways. The minimum it requires is the first argument,
`thing_to_print`:

```python
print_something('something')
```

```output
something
```

The extra arguments can be overridden from their default values. They can be set as **positional
arguments**, in which case the order matters:

```python
print_something('something', 2, True)
```

```output
SOMETHING
SOMETHING
```

Or as **named arguments**, in which case order doesn't matter:

```python
print_something(upper_case=True, number_of_times=2, thing_to_print='something')
```

```output
SOMETHING
SOMETHING
```

Or even as a combination. Positional arguments must be provided first, in order, followed
by named arguments in any order:

```python
print_something('something', upper_case=True)
```

```output
SOMETHING
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Functions

Write a function that can take any CSV file, and provided that it is structured similarly to
combined_tidy_vcf.csv, plot coverage against genomic position for each sample in the data. Then,
call the function on combined_tidy_vcf.csv.

A few hints:

- You'll need the function to take an argument corresponding to the name of the file to load
- You can read a CSV file into a data frame with `pandas.read_csv()`

:::::::::::::::  solution

## Solution

```python
def plot_coverage(csv_file):
   variants = pandas.read_csv(csv_file)

   fig = plt.figure(figsize=(12, 4))
   plot_number = 1
   samples = variants['sample_id'].unique()
   total_plots = len(samples)

   for sample in samples:
      plt.subplot(1, total_plots, plot_number)
      sample_data = variants[variants['sample_id'] == sample]
      plt.scatter(sample_data['POS'], sample_data['MQ'], s=12, alpha=0.5, edgecolors='none')
      plt.title(sample)
      plot_number = plot_number + 1

   fig.suptitle('Mapping quality vs. position')
   fig.supxlabel('Position')
   fig.supylabel('Mapping quality')


plot_coverage('combined_tidy_vcf.csv')
```

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::

## Further reading

We briefly mentioned writing Python code as **scripts** earlier on. We could put the
function that we wrote above into a script, and then run it with whatever CSV file we want
to provide as a command line argument. To do this, we can use:

- [`sys.argv`](https://docs.python.org/3/library/sys.html#sys.argv) - a list that can be accessed
  via `import sys`, that contains all the arguments that were provided on the command line
- ['argparse'](https://docs.python.org/3/library/argparse.html) - a package that comes as part
  of the Python **standard library**, i.e. it's not useable until you run `import argparse`,
  but you don't have to install it independently like with Pandas or Matplotlib. It has many features
  for reading command line arguments and making full-featured, professional command-line interfaces.
  It also has extensive documentation and a [tutorial](https://docs.python.org/3/howto/argparse.html#argparse-tutorial)
  for learning how to use it.

:::::::::::::::::::::::::::::::::::::::: keypoints

- matplotlib is a powerful and flexible library for high-quality plots
- Common tasks can be combined with conditionals, for loops and functions to
  accommodate different scenarios more flexibly and with less code

::::::::::::::::::::::::::::::::::::::::::::::::::
