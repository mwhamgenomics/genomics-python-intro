---
title: 'Introduction to Python'
teaching: 90
exercises: 30
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is Python, and what can it do?
- Why use it?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explore some of Python's basic functionality
- Use variables to store data
- Compare two methods of interacting with Python: scripts and interactive sessions
- Find out how to get help with Python's built-in functions

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Python is an open-source, **general purpose** programming language, with a wide range of features making it suited to many applications.
It had its first release in 1991, Python 2.0 was released in 2000, and Python 3.0 in 2008.

Python has many advantages to programmers of all experience levels:

- It is one of the world's most popular programming languages. This means that it is widely supported - indeed, it is included as standard in many operating systems.
- It has a large and active community, meaning that there are many places out there to get help.
- Python is a **high-level** programming language. It has many features for abstraction, insulating the user from the low-level complexity of the individual 1s and
  0s being manipulated. This allows us to work with relatively user-friendly concepts such as strings and lists. There are a number of benefits of this:
  - Speed of development. It is considerably faster to write a program in Python than it would be to write an equivalent program in a less abstracted language like C++.
  - Readability. Python's syntax is designed to be highly readable, meaning that it's easier for people to understand and work with your code - including yourself in six months' time!
- Python has very good **error reporting**, and it's generally quite easy to find faults.
- Python can be obtained free of charge.
- Python is open-source. Its development is community-driven, and the Python project has a lot of eyes on it, constantly identifying fixes and making improvements.

Python is also highly versatile. Unlike specialised languages such as R, MatLab, SASS or Nextflow, Python is suitable for a wide range of tasks, including:

- Data science
- Web development
- Automation
- Video game development
- Multimedia
- Embedded systems

In the [R for Genomics](https://datacarpentry.github.io/genomics-r-intro) course we use R, which is specifically optimised for working with tabular data. Python can
do the same tasks by using packages - add-ons that members of the community have contributed.


## Running Python interactively

If running Python in Jupyter, go to the Launcher and select 'Notebook' -> 'Python 3 (ipykernel)'. If running Python on the plain command line, run the terminal command:

    python

You should now have an input box in which you can type commands. Commands can be entered by selecting 'Run' in Jupyter, or by hitting Enter on the plain command line.


## Hello world

Let's start with an exercise done by first-time programmers all over the world. Enter and run the command:

    print('Hello world')

You should see the text 'Hello world' displayed on the console's output. It may not look like much, but several things just happened:

- You have created a **string**
- You have **called** a **function** by provided this string as an **argument**
- The function has produced some output


## Running Python from a file

Let's try running Python a different way. In the Jupyter launcher, go to 'Other' -> 'Python file'. This will open a text file. On the plain command line, use your text
editor of choice to create a new file.

In the new file, add the same command we used before:

```python
print('Hello world')
```

Now save this file - Python source code files like this are usually saved with a `.py` file extension.

Now to run it. Start up a terminal session locally, or in Jupyter by going to 'Other' -> 'Terminal' in Jupyter. Supposing you saved your file as `hello_world.py`, run it
with the command:

    python hello_world.py

You should see the same output displayed in the terminal.

Although we will not be using this method any further in this course, this is the most common way Python is used in the wild.


## Variables

In order to manipulate pieces of data, we need a way of storing them in memory. We can do this with **variables**. Variables are assigned with three elements:

- A name that you want to call the variable. This can be anything you want - with a few provisos.
- The `=` symbol
- The value you want to assign to the variable

Let's try assigning a **string**, like the 'hello world' example from before:

```python
some_text = 'Hello world'
```

In Python, strings are always marked with either single quotes or double quotes. Python doesn't mind which you use, as long as a string begins and ends with the same type of quote.

Now that this data is stored as a variable, we can do things with it, such as passing it to the `print` function like we did before:

```python
print(some_text)
```

## Other data types

We've tried strings, but let's try it with some other types of data. 

::::::::::::::::::::::::::::::::::::: challenge 

## Numbers

Try assigning the following pieces of data to a variable and printing it:

- An integer, e.g. 1
- A decimal number, e.g. 3.14 (also known in programming as a **float**)

:::::::::::::::::::::::: solution 

```python
an_integer = 1
a_float = 3.14

print(an_integer)
print(a_float)
```

```output
1
3.14
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


## Combining data together

Python is able to do basic arithmetic operations on numbers, using the `+`, `-`, `*` and `/` symbols:

```python
print(1 + 1)
print(10 * 3)
print(10 / 3)
```

Some of these **operators** also work on strings:

```python
some_text = 'some' + 'text'
print(some_text)
```

```output
sometext
```

Here we use the same `+` symbol we used on numbers above, expect here we **concatenate** the two strings together.

::::::::::::::::::::::::::::::::::::: challenge 

## 1 vs. '1'

What happens when you use the `+` operator on the following?

- the numbers 1 and 2 (i.e. `1 + 2`)
- the strings '1' and '2' (i.e. `'1' + '2'`)

:::::::::::::::::::::::: solution 

There is a difference between the number `1` and the string `'1'`, as can be seen when we try to perform operations on them:

```python
print(1 + 2)
print('1' + '2')
```

```output
3
12
```

:::::::::::::::::::::::::::::::::


## Operations on dissimilar data types

What happens when you use the `+` operator on the following?

- an integer and a float, e.g. `1 + 2.3`
- a number and a string, e.g. `1 + '2'`

:::::::::::::::::::::::: solution 

For the `+` operator, numerical types are compatible with each other, however strings and numbers are not:

```python
print(1 + 2.3)
print(1 + '2')
```

```output
3.3
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


## Reassigning variables

We can also overwrite variables. Here, we create an integer variable and then increment it:

```python
an_integer = 0
an_integer = an_integer + 1
print(an_integer)
```

```output
1
```


## Lists

So far we have been storing pieces of data individually, however we can also store **lists** of them:

```python
some_list = [1, 3.14, 'some', 'text']
```

A list is denoted by square brackets, and each item is separated by a comma. Lists can store any type of data (even other lists!),
although in the wild you will usually work with lists of strings, lists of integers, etc.


### Indexing

Once we have stored a list, we can retrieve items out of them by **indexing**, using square brackets containing the **index** of the
item that we want to retrieve:

```python
print(some_list[1])
```

However, the result of running this may be unexpected:

```output
3.14
```

Despite seemingly asking for the first item, we have been given the second. This is because Python is **zero-indexed** - it counts from 0.
So to retrieve the first item, we should do:

```python
print(some_list[0])
```

::::::::::::::::::::::::::::::::::::: challenge 

## Array indexing

Create a list of four items:

```python
items = ['a', 'list', 'of', 'strings']
```

What happens if you try to retrieve an item at position 10?

:::::::::::::::::::::::: solution 

```python
print(items[10])
```

Trying to retrieve an item as position 10 from a list that only has four items results in an error message:

```output
IndexError: list index out of range
```

:::::::::::::::::::::::::::::::::

## Negative indexing

What happens if you try to retrieve an item at position -1?

:::::::::::::::::::::::: solution

```python
print(items[-1])
```

Negative indexes count backwards from the end. Position `-1` will always retrieve the last item:


```output
strings
```

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

### Slicing

We can also retrieve **slices** of a string by specifying two numbers in the square brackets, separated by a colon:

```python
items = ['a', 'list', 'of', 'strings']
print(items[0:2])
```

These two numbers specify where the slice should start and end. Running this command produces:

```output
['a', 'list']
```

This may again be surprising. If Python counts from 0, why has the `'of'` at position 2 not been included? Slicing in Python
works by starting at the first index and going up to, **but not including**, the second index.

There is logical reasoning behind this, namely the size of the resulting slice will be equal to the difference between the two indexes.
Although perhaps not apparent right now, this is useful when using Python in the wild.

When indexing, numbers can also be left out. In this case, Python will default to using the start or end of the list,
depending on which number is omitted:

```python
print(items[:2])
# prints ['a', 'list']
```

Interestingly, we can also omit both numbers, which will return the whole list:

```python
print(items[:])
# prints ['a', 'list', 'of', 'strings']
```

::::::::::::::::::::::::::::::::::::: challenge 

## Indexing and slicing strings

Create a long string variable, e.g:

```python
long_string = 'a very long string with lots of characters'
```

What happens if you try to index and slice it as if it were a list?

:::::::::::::::::::::::: solution 

Strings can be indexed and sliced exactly the same way as lists!

```python
print(long_string[0])
print(long_string[-1])
print(long_string[2:5])
```

In Python, a string is just that - an ordered series of characters. That doesn't mean that strings and lists
are the same in all respects, but it does mean that many features of the language (indexing, slicing, mathematical
operators and other kinds of operator) can apply to many different data types.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


## Types and casting

So far we have only used one function, `print()`. Let's introduce another - `type()`:

```python
some_number = 3.14
type_of_some_number = type(some_number)
print(type_of_some_number)
```

This produces:

```output
<class 'float'>
```

The `type()` function's **return value** is the data type of whatever is input to it.


### Aside: nested functions

In the example above, we did each operation one line at a time. We saved a number to a variable, then we called `type()` and
saved its result to another variable, and then we printed it.

There are, however, many ways to solve a problem. Another way of doing it is:

```python
some_number = 3.14
print(type(some_number))
```

Here, we skipped creating the second variable, and passed the return value of the `type()` call straight to `print()`. This shows
that function calls can be `**nested**.

Indeed, we can do the whole thing in one line if we want to:

```python
print(type(3.14))
```

The above approaches all do the same thing logic-wise - the only real difference is what the code looks like on-screen. Breaking
things over multiple lines or nesting function calls inside each other is a choice for the programmer to make, and there is a balance
to be made regarding readability, between being **verbose** and being **concise**.


## Casting

Often when dealing with data, we need to be able to convert something from one type to another. This process is known as **casting**.
There are several casting functions, named after the data type that they convert to:

- `int()`
- `float()`
- `str()`

::::::::::::::::::::::::::::::::::::: challenge

## Casting

Try running each of the above casting functions on the following inputs:

- an integer, e.g. `1`
- a float, e.g. `3.14`
- a string resembling a number, e.g. `'10'`
- a string resembling some text, e.g. `'ten'`

:::::::::::::::::::::::: solution

Some of these will work, and some won't:

```python
int(1)      # 1
int(3.14)   # 3
int('10')   # 10
int('ten')  # ValueError: invalid literal for int() with base 10: 'ten'

float(1)      # 1.0
float(3.14)   # 3.14
float('10')   # 10.0
float('ten')  # ValueError: could not convert string to float: 'ten'

str(1)      # '1'
str(3.14)   # '3.14'
str('10')   # '10'
str('ten')  # 'ten'
```

:::::::::::::::::::::::::::::::::

## Casting to a list

There is also a `list()` function. What happens if you try running this on the above values?

:::::::::::::::::::::::: solution

Numbers cannot be cast to lists, and will result in TypeError messages. However, it will have an effect on strings:

```python
list('10')   # ['1', '0']
list('ten')  # ['t', 'e', 'n']
```

Another way of demonstrating that strings and lists have some similarities, in that a string is an ordered series of characters.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


## Booleans

Python is also able to represent true/false values. Some languages repurpose the numbers 0 and 1, but in Python this can be done with:

```python
is_the_sky_blue = True
is_the_earth_banana_shaped = False
```

The two boolean values are `True` and `False` (capitalised), and also have their own casting function, `bool()`.



## Other data types

Python has a wide range of other useful data types and structures, including bytes, dictionaries, sets and tuples. However, it is not
necessary to know all of them right now, and they are considered beyond the scope of this course.


## Control flow

Control flow is a central part of programming. We often need our code to be able to make choices and respond to different situations. This
can be done with an **if/else statement**, also known as a **conditional**. Suppose we want different things to happen depending on how
large a number is:

```python
x = 150

if x > 100:
    print('x is large')
```

In Python, an if/else statement always consist of at least:

- The keyword `if`
- A conditional statement
- A colon to end the first line
- An indented block of code afterwards, that will run if the condition is true. You can indent with tabs or spaces (many text editors will
  automatically convert a tab to four spaces), and with any number of them, as long as every line in the block is indented consistently.

In the example above, we also introduce the `>` operator, which will return `True` or `False` depending on whether the left hand number is
greater than the right hand number. There are also:

- `==` - equal
- `!=` - not equal
- `<` - less then
- `>=` - greater than or equal
- `<=` - less than or equal

An if/else statement can also have an `else` block, that will execute if the condition is not true:

```python
x = 150

if x > 100:
    print('x is large')
else:
    print('x is small')
```

It is also possible to have many branching pathways using `elif`:

```python
x = 150

if x > 1000:
    print('x is huge')
elif x > 500:
    print('x is very big')
elif x > 100:
    print('x is large')
else:
    print('x is small')
```

Finally, it is possible to chain together multiple conditional statements with the keywords
`and` and `or`:

```python
some_value = 'this'

if some_value == 'this' or some_value == 'that':
    print(some_value)
```

::::::::::::::::::::::::::::::::::::: challenge

## If/else statements

Try running the above if/elif/else statement yourself, with `x` set to different numbers. How many printouts
do you get each time you run it?

:::::::::::::::::::::::: solution

You should only get one line printed each time you run the statement. Even if a condition satisfies multiple conditions,
only the **first condition found to be true** will execute.

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


## Loops

One of the main problems we solve in being able to program is processing large amounts of data. We can do this using **loops**, also
known as **iteration**.

The most common type of loop encountered in programming (especially in Python) is the **for loop**:

```python
a_list = ['one', 'two', 'three', 'four']

for item in a_list:
    print(item)
```

A `for` loop consists of:

- The keyword `for`
- A **loop variable** to contain the current item being processed
- The keyword `in`
- An **iterable** to iterate through
- A colon to end the first line
- An indented block of code, that will run once for each item in the iterable.

Running the loop above will result in:

```output
one
two
three
four
```

We get this because Python has iterated through the four items of the list, and run the print statement on each one.

::::::::::::::::::::::::::::::::::::: challenge

## Combining loops and conditionals

Using loops and conditionals together is a powerful way of taking a collection of values and processing each one
differently depending on certain rules.

Create a variable consisting of a list of some numbers between 1 and 100, e.g:

```python
a_list = [5, 76, 23, 15, 60, 50, 89]
```

Then loop through this list and print any items that are above 50.

Hint: both for loops and conditionals result in an indented code block. Since you will be combining the two, you
will have multiple layers of nesting.

:::::::::::::::::::::::: solution

```python
a_list = [5, 76, 23, 15, 60, 50, 89]

for current_number in a_list:
    if current_number > 50:
        print(current_number)
```

This should result in:

```output
76
60
89
```

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


## What about writing our own functions?

So far we have used pre-defined functions built into Python, such as `print()`, `type()` and the casting functions. It is also possible to
make our own, and indeed this allows us to organise our code effectively in order to tackle large tasks and solve large problems in the wild.
We'll take a look at this at the end of this course.


## Getting help

We've discussed many aspects of using Python, but how were we supposed to know these things if we didn't have this course, and how can we learn more?

Official documentation for all aspects of Python's functions, syntax and ecosystem can be found at https://docs.python.org. If you need help with a
specific function, you can run a function called `help()`. For example, to display the documentation for the `print()` function:

```python
help(print)
```

::::::::::::::::::::::::::::::::::::: keypoints 

- Python is a general-purpose programming language able to solve a wide range of problems
- We can work with many different types of data
- Lists and strings can be indexed and sliced
- We can process different values in different ways using conditional statements
- We can process large collections of values using for loops
- We can get help on Python's official website, or with the `help()` function
::::::::::::::::::::::::::::::::::::::::::::::::
