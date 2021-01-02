## Divide & Conquer Algorithms

Notes of Divide and Conquer, Sorting and Searching

### Randomized Selection

**Problem** - Selecting *i'th* minimum element from an array.

**Naive Solution** - First sort then choose *i'th* element of the array.
Time Complexity - $\mathcal{O}(n\log n)$

**Better Solution**
```python
from random import randint
def RSelect(arr, i):
  n = len(arr)
  if n == 1:
    return arr[0]
  pivot = randint(0, n-1)
  if pivot + 1 == i:
    return arr[pivot]
  elif pivot + 1 > i:
    return RSelect(arr[:pivot], i)
  else:
    return RSelect(arr[pivot:], i - pivot)
```



