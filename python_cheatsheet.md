#Python Cheatsheet

##Installing python 
Linux comes with pre-installed python interpreter, no need to install, and default interpreter is python 2.7 in most of the 
linux distribution

```bash
$ python #This will open python 2.7 interpreter
```

```bash
$ python3 #This will open python3 interpreter
```
> I recommend to use python3 as the community is much more focused on creating new libraries for python3, python2 has a supports
only bux fixes and security patches.

> This cheatsheet is much more focused on python3 

```python
from __future__ import python
3 / 2 equals to 1.5
3.0 / 2 equals 1.5

$ python -Qnew
>>> 3 / 2 => 1.5

##Difference between expression and statement


##Some important built in functions

```python
abs(-10)
round(12.3) => round to the nearest value
round(12.5) => 13.0
import math
math.floor(33.9) => 33.0
math.ceil(33.2) => 34.0
math.sqrt(9) => 3.0
```

##Some shortcuts methods in python

```python
foo = math.sqrt
foot(3)
```

##importing the standard library

```python
from math import floor
from math import ceil as c
from math import sqrt

>>> sqrt(-1) # It will throw the ValueError, in some platfrom it show nan ie. not a number
>>> c(33.2) # 34.0
```

```python
from math import floor
f = floor
f(33.9) #32
```

##Cmath module

```python
from cmath import sqrt
sqrt(-1) #1j
>>> (1 + 2j) * (1 - 3j)
```

> note: There are no seperate imaginary in python, they are treated as a complex number whose real component is zero

##Diffrence between repr and str
```python
print(str("hello world"))
print (repr("hello world"))
```

STR and REPR is used to print the values, the main difference is repr disply the value as a legal python expression.
```python
x = 2
repr(x) == `x`
```
```python
>>> print(" The value of x is " + str(x))
>>> print("The value of x is " + repr(x))
>>> print "The value of x is" + `x`
```

The statement 1 and 2 are same
The backticks are removed in python 3 so used repr(x) method.

