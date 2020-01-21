---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.1'
      jupytext_version: 1.2.4
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

# Gregory Aitken


**Instructions:** This is an individual assignment, but you may discuss your code with your neighbors.


# Python and NumPy

While other IDEs exist for Python development and for data science related activities, one of the most popular environments is Jupyter Notebooks.

This lab is not intended to teach you everything you will use in this course. Instead, it is designed to give you exposure to some critical components from NumPy that we will rely upon routinely.

## Exercise 0
Please read and reference the following as your progress through this course. 

* [What is the Jupyter Notebook?](https://nbviewer.jupyter.org/github/jupyter/notebook/blob/master/docs/source/examples/Notebook/What%20is%20the%20Jupyter%20Notebook.ipynb#)
* [Notebook Tutorial](https://www.datacamp.com/community/tutorials/tutorial-jupyter-notebook)
* [Notebook Basics](https://nbviewer.jupyter.org/github/jupyter/notebook/blob/master/docs/source/examples/Notebook/Notebook%20Basics.ipynb)

**In the space provided below, what are three things that still remain unclear or need further explanation?**


I'm familiar with Jupyter Notebook already from DATA 301 :)


## Exercises 1-7
For the following exercises please read the Python appendix in the Marsland textbook and answer problems A.1-A.7 in the space provided below.


## Exercise 1

```python
# Problem A.1 Make an array a of size 6 × 4 where every element is a 2.
import numpy as np
a = np.full((6, 4), 2)
print(a)

# alternate method
# a = np.ones((6, 4), int) * 2
# print(a)
```

## Exercise 2

```python
# Problem A.2 Make an array b of size 6 × 4 that has 3 on the leading diagonal and 1
# everywhere else. (You can do this without loops.)
b = np.ones((6, 4), int)
np.fill_diagonal(b, 3)
print(b)

# alternate method
# b = np.ones((6, 4), int)
# b[range(4), range(4)] = 3
# print(b)
```

## Exercise 3

```python
# Problem A.3 Can you multiply these two matrices together? Why does a * b work, but
# not dot(a,b)?

# a * b is element-wise multiplication, so it works because the dimensions match
a * b

# np.dot(a, b) is matrix multiplication, so it doesn't work because the "inner" dimensions must match
# np.dot(a, b)
```

## Exercise 4

```python
# Problem A.4 Compute dot(a.transpose(),b) and dot(a,b.transpose()). Why are
# the results different shapes?
display(np.dot(a.transpose(), b))
display(np.dot(a, b.transpose()))
```

The resulting matrix has the shape of the "outer" dimensions


## Exercise 5

```python
# Problem A.5 Write a function that prints some output on the screen and make sure you can
# run it in the programming environment that you are using.
def bla():
    print("bla bla bla")

bla()
```

## Exercise 6

```python
# Problem A.6 Now write one that makes some random arrays and prints out their sums, the mean value, etc.
def rand_array_math():
    a = np.random.randint(0, 10, (3, 3))
    b = np.random.randint(0, 10, (3, 3))
    c = np.random.randint(0, 10, (3, 3))
    
    print("a:\n", a)
    print("b:\n", b)
    print("c:\n", c)
    
    print("a+b:\n", a+b)
    print("b+c:\n", b+c)
    print("a+c:\n", a+c)
    
    print("mean of a:", np.mean(a))
    print("mean of b:", np.mean(b))
    print("mean of c:", np.mean(c))

rand_array_math()
```

## Exercise 7

```python
# Problem A.7 Write a function that consists of a set of loops that run through an array and count the number of ones in it.
# Do the same thing using the where() function (use info(where) to find out how to use it).
def count_ones(array):
    ones = 0
    for row in array:
        for val in row:
            if val == 1:
                ones += 1
    return ones

a = np.random.randint(0, 3, (3, 3))
print(a)
count_ones(a)
```

```python
# where method
np.where(a == 1, 1, 0).sum()
```

```python
# alternate where method
len(np.where(a == 1)[0])
```

## Excercises 8-???
While the Marsland book avoids using another popular package called Pandas, we will use it at times throughout this course. Please read and study [10 minutes to Pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/10min.html) before proceeding to any of the exercises below.


## Exercise 8
Repeat exercise A.1 from Marsland, but create a Pandas DataFrame instead of a NumPy array.

```python
# Problem A.1 Make an array a of size 6 × 4 where every element is a 2.
import pandas as pd
a = pd.DataFrame(np.full((6, 4), 2))
a
```

## Exercise 9
Repeat exercise A.2 using a DataFrame instead.

```python
# Problem A.2 Make an array b of size 6 × 4 that has 3 on the leading diagonal and 1
# everywhere else. (You can do this without loops.)
b = pd.DataFrame(np.ones((6, 4), int))
b.iloc[range(4), range(4)] = 3
b
```

## Exercise 10
Repeat exercise A.3 using DataFrames instead.

```python
# Problem A.3 Can you multiply these two matrices together? Why does a * b work, but
# not dot(a,b)?

# a * b is element-wise multiplication, so it works because the dimensions match
a * b

# a.dot(b) is matrix multiplication, so it doesn't work because the "inner" dimensions must match
# a.dot(b)
```

## Exercise 11
Repeat exercise A.7 using a dataframe.

```python
# Problem A.7 Write a function that consists of a set of loops that run through an array
# and count the number of ones in it. Do the same thing using the where() function
# (use info(where) to find out how to use it).

def count_ones_df(df):
    ones = 0
    for i in range(df.shape[0]):
        for j in range(df.shape[1]):
            if df.iloc[i, j] == 1:
                ones += 1
    return ones

a = pd.DataFrame(np.random.randint(0, 5, (5, 5)))
print(a)
count_ones_df(a)
```

```python
# where method
a.where(a == 1, 0).to_numpy().sum()
```

```python
# alternate where method
a.where(a == 1, 0).sum().sum()
```

## Exercises 12-14
Now let's look at a real dataset, and talk about ``.loc``. For this exercise, we will use the popular Titanic dataset from Kaggle. Here is some sample code to read it into a dataframe.

```python
titanic_df = pd.read_csv(
    "https://raw.githubusercontent.com/dlsun/data-science-book/master/data/titanic.csv"
)
titanic_df
```

Notice how we have nice headers and mixed datatypes? That is one of the reasons we might use Pandas. Please refresh your memory by looking at the 10 minutes to Pandas again, but then answer the following.


## Exercise 12
How do you select the ``name`` column without using .iloc?

```python
titanic_df["name"]
```

```python
titanic_df.name
```

## Exercise 13
After setting the index to ``sex``, how do you select all passengers that are ``female``? And how many female passengers are there?

```python
titanic_df.set_index('sex',inplace=True)
titanic_df
```

```python
titanic_df.loc["female"]
```

```python
len(titanic_df.loc["female"])
```

## Exercise 14
How do you reset the index?

```python
titanic_df.reset_index()
```
