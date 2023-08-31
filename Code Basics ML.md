Only Concepts are Here. Codes are in different dir.
## Lecture 2 :

```python
import pandas as pd
import numpy as np
import matplot.pyplot as plt
from sklearn import linear_model
```

### Pandas Basics

![[Pandas_Cheat_Sheet.pdf]]

### Numpy Basics

- The NumPy library contains multidimensional array and matrix data structures with methods to efficiently operate on it. 
- NumPy can be used to perform a wide variety of mathematical operations on arrays. 

```python
# 1D Array
a = np.array([1, 2, 3, 4, 5, 6])

# 2D Array
a = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
```

- In NumPy, dimensions are called **axes**.


#### How to create a basic array

```python
np.zeros(2)
# array([0., 0.])

np.ones(2)
# array([1., 1.])

# Create an empty array with 2 elements
np.empty(2) 
# array([3.14, 42.  ])  # may vary

np.arange(4)
# array([0, 1, 2, 3])

np.arange(2, 9, 2)
# array([2, 4, 6, 8])

# `np.linspace()` to create an array with values that are spaced linearly in a specified interval
np.linspace(0, 10, num=5)
# array([ 0. ,  2.5,  5. ,  7.5, 10. ])

# Specifying Data Type
x = np.ones(2, dtype=np.int64)
x
# array([1, 1])
```


### Adding, removing, and sorting elements

```python
# Sort
arr = np.array([2, 1, 5, 3, 7, 4, 6, 8])
np.sort(arr)
# array([1, 2, 3, 4, 5, 6, 7, 8])

# Concatenate
np.concatenate((a, b))
# array([1, 2, 3, 4, 5, 6, 7, 8])


```















## Lecture 5 :

### Save Trained Model using Joblib and Pickle

- If too many numpy arrays are present, Joblib is more efficient.
- Both have same functionality.

#### Pickle

```python
import pickle

with open('model_pickle','wb') as f:
	pickle.dump(model,f)

with open('model_pickle','rb') as f :
	mp = picle.load(f)

mp.predict(5000)
```

#### Joblib

```python
from sklearn.externals import joblib

joblib.dump(model,'model_joblib')

mj = joblib.load('model_joblib')

mj.predict(5000)
```


## Lecture 7 : Training & Testing Data

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.2)


# It will take random 80% data for training model. without random_state para it will change everytime.
# To get a same Random state everytime : ->
train_test_split(X,y,test_size=0.2,random_state=10)

# Training
from sklearn.linear_model import LinearRegression
clf = LinearRegression()
clf.fit(X_train,y_train)

# Testing
clf.predict(X_test)

# Score or Accuracy of Model
clf.score(X_test,y_test)
```




## Image Classification

### Part 4 -> Feature Engineering

#### Frequency Concept in an Image

- Basic -> https://www.youtube.com/watch?v=xrTor1uw5iI

#### Fourier Transform

- Source -> https://www.youtube.com/watch?v=spUNpyF58BY