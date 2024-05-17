<!-- badges: start -->
[![PyPI version](https://badge.fury.io/py/gower.svg)](https://pypi.org/project/gower-multiprocessing/)
[![Downloads](https://pepy.tech/badge/gower/month)](https://pepy.tech/project/gower-multiprocessing/month)
<!-- badges: end -->

# Introduction

Gower's distance calculation in Python. Gower Distance is a distance measure that can be used to calculate distance between two entity whose attribute has a mixed of categorical and numerical values. [Gower (1971) A general coefficient of similarity and some of its properties. Biometrics 27 857–874.](https://www.jstor.org/stable/2528823?seq=1) 

More details and examples can be found on my personal website here:(https://www.thinkdatascience.com/post/2019-12-16-introducing-python-package-gower/)

Core functions are wrote by [Marcelo Beckmann](https://sourceforge.net/projects/gower-distance-4python/files/).

Multiprocessing added by [Szymon Bobek](https://github.com/sbobek)

# Examples

## Installation

```
pip install gower-multiprocessing
```

## Generate some data

```python
import numpy as np
import pandas as pd
import gower-multiprocessing as gower

Xd=pd.DataFrame({'age':[21,21,19, 30,21,21,19,30,None],
'gender':['M','M','N','M','F','F','F','F',None],
'civil_status':['MARRIED','SINGLE','SINGLE','SINGLE','MARRIED','SINGLE','WIDOW','DIVORCED',None],
'salary':[3000.0,1200.0 ,32000.0,1800.0 ,2900.0 ,1100.0 ,10000.0,1500.0,None],
'has_children':[1,0,1,1,1,0,0,1,None],
'available_credit':[2200,100,22000,1100,2000,100,6000,2200,None]})
Yd = Xd.iloc[1:3,:]
X = np.asarray(Xd)
Y = np.asarray(Yd)

```

## Find the distance matrix

```python
gower.gower_matrix(X)
```




    array([[0.        , 0.3590238 , 0.6707398 , 0.31787416, 0.16872811,
            0.52622986, 0.59697855, 0.47778758,        nan],
           [0.3590238 , 0.        , 0.6964303 , 0.3138769 , 0.523629  ,
            0.16720603, 0.45600235, 0.6539635 ,        nan],
           [0.6707398 , 0.6964303 , 0.        , 0.6552807 , 0.6728013 ,
            0.6969697 , 0.740428  , 0.8151941 ,        nan],
           [0.31787416, 0.3138769 , 0.6552807 , 0.        , 0.4824794 ,
            0.48108295, 0.74818605, 0.34332284,        nan],
           [0.16872811, 0.523629  , 0.6728013 , 0.4824794 , 0.        ,
            0.35750175, 0.43237334, 0.3121036 ,        nan],
           [0.52622986, 0.16720603, 0.6969697 , 0.48108295, 0.35750175,
            0.        , 0.2898751 , 0.4878362 ,        nan],
           [0.59697855, 0.45600235, 0.740428  , 0.74818605, 0.43237334,
            0.2898751 , 0.        , 0.57476616,        nan],
           [0.47778758, 0.6539635 , 0.8151941 , 0.34332284, 0.3121036 ,
            0.4878362 , 0.57476616, 0.        ,        nan],
           [       nan,        nan,        nan,        nan,        nan,
                   nan,        nan,        nan,        nan]], dtype=float32)


## Find Top n results

```python
gower.gower_topn(Xd.iloc[0:2,:], Xd.iloc[:,], n = 5)
```




    {'index': array([4, 3, 1, 7, 5]),
     'values': array([0.16872811, 0.31787416, 0.3590238 , 0.47778758, 0.52622986],
           dtype=float32)}


## Performance comparison with single-process version

```
Single process (DS-size: 10000, time:   15.58 sec.)	█
Multi process  (DS-size: 10000, time:    2.93 sec.)	

Single process (DS-size: 20000, time:   54.30 sec.)	█████
Multi process  (DS-size: 20000, time:   11.57 sec.)	█

Single process (DS-size: 30000, time:  119.80 sec.)	███████████
Multi process  (DS-size: 30000, time:   24.86 sec.)	██

Single process (DS-size: 40000, time:  202.65 sec.)	████████████████████
Multi process  (DS-size: 40000, time:   41.77 sec.)	████

Single process (DS-size: 50000, time:  318.64 sec.)	███████████████████████████████
Multi process  (DS-size: 50000, time:   68.36 sec.)	██████

Single process (DS-size: 60000, time:  469.64 sec.)	██████████████████████████████████████████████
Multi process  (DS-size: 60000, time:   96.24 sec.)	█████████

Single process (DS-size: 70000, time:  653.27 sec.)	█████████████████████████████████████████████████████████████████
Multi process  (DS-size: 70000, time:  143.31 sec.)	██████████████

Single process (DS-size: 80000, time:  857.04 sec.)	█████████████████████████████████████████████████████████████████████████████████████
Multi process  (DS-size: 80000, time:  181.60 sec.)	██████████████████

Single process (DS-size: 90000, time: 1129.21 sec.)	████████████████████████████████████████████████████████████████████████████████████████████████████████████████
Multi process  (DS-size: 90000, time:  252.36 sec.)	█████████████████████████
```

