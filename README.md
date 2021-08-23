# pyminfunc
version 0.9 (Aug 23 2021)

Reimplementation of Mark Schmidt's minFunc in python.
(https://www.cs.ubc.ca/~schmidtm/Software/minFunc.html)

*Note:* Do **not** use the Matlab code for minFunc here.  It has been modified 
to allow for easy testing of equivalence between this python implementation and Mark Schmidt's original Matlab code.  If you want Schmidt's Matlab code, go to the above URL.

## Purpose

This code is intended to be a reimplementation of Mark Schmidt's `minFunc` 
(which is in Matlab/Octave) multi-variate unconstrained minimization package
in python using numpy and scipy.  The goal is not to change the algorithms
in minFunc, but just to provide their equivalences in python.

## Status

The code has been tested for most of the paths through the algorithms on a handful of optimization problems (none very large).  It does *not* reproduce the results exactly (down to the last floating-point bit), mainly because 1) operations like Cholesky decompositions are performed differently in scipy and Matlab, and
2) even operations like inner product can produce different results due to floating point variablity (see the README in the testing directory).

On the tests performed, this python version does implement the same algorithm.  Small differences can accumulate and cause different optimization paths, but these (appear to) only happen at points where the underlying method is slightly unstable and one shouldn't expect to rely on the exact answer.

## How to use

Currently this is not a python package (yes, that's a "to do").  The files `minFunc.py`, `lbfgsutil.py`, `autodif.py`, and `util.py` are sufficient to run the python version.  If placed in the same directory as the python code that uses them, the following example demonstrates how to use it.

```python
from minFunc import minFunc
import numpy as np

def myf(x):
     sinx1 = np.sin(x[1])
     cosx1 = np.cos(x[1])
     f = x[0]**2 * sinx1**2 + (x[1]-10)**2 + (x[0]-10)**2
     g = np.array([2*x[0] * sinx1**2 + (x[0]-10),
                   x[0]**2 * sinx1 * cosx1 + (x[1]-10)])
     return f,g

x0 = np.array([0,0])
options = {'display':'off'} # many more options possible
xmin,fmin,exitflag = minFunc(myf,x0,options)
print(xmin)
```

The options are supplied as a dictionary, as opposed to a structure (this is the
only real change in the interface.  For full set of options, see Mark Schmidt's code at the moment.

## Bugs

Currently, I'm sure there are paths through the code that have not been fully tested.
If you find a bug, please report it.  I will fix them as I get time.  A **minimal** example that reproduces the bug will be necessary for me to fix it.
