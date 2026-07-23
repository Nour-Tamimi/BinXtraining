
# Day 3 — What I Learned: NumPy Arrays

Today's focus was NumPy, and it's the first day where I felt the "why is this better than plain Python" click for real.

## What I did
I created arrays a handful of different ways (from a list, zeros, ones, a range, evenly spaced values, random values), then practiced checking their shape and dtype. I worked through indexing and slicing on a 2D array, used a boolean condition to filter values, and tried broadcasting by adding a 1D array onto every row of a 2D array.

## What clicked for me
- The speed argument for NumPy isn't abstract to me anymore. Multiplying every element of a Python list by 2 needs a loop or a list comprehension; with a NumPy array it's just `a * 2`. Once I saw that, the earlier lesson about "vectorized operations replace loops" from Day 2 actually made sense in practice, not just in theory.
- Boolean masking was the biggest "aha" of the day — `a[a > 2]` reads almost like a sentence ("give me a, where a is greater than 2"), and I can already see how useful this will be for filtering real data instead of writing manual if-checks row by row.
- Broadcasting took a couple of tries to get right. My first instinct was that adding a 1D array to a 2D array should error out because the shapes don't match, but NumPy stretches the smaller array to fit as long as the last dimension lines up. I had to run a small example and print the result to actually believe it worked.

## Code I want to remember
```python
import numpy as np

m = np.array([[1, 2, 3], [4, 5, 6]])
print(m[:, 1])       # column, not row — took me a second to get used to this
print(m[m > 2])      # boolean masking

matrix = np.array([[1, 2, 3], [4, 5, 6]])
row = np.array([10, 20, 30])
print(matrix + row)  # broadcasting: row gets added to every row of matrix
```

## Still a bit fuzzy
- I want more practice with broadcasting rules for shapes that *don't* line up cleanly — today's example worked, but I haven't yet run into (and debugged) a real shape-mismatch error, and I know that's coming.
- `.dtype` and how NumPy silently upcasts types (e.g., ints becoming floats) when arrays mix data — I noticed it happen once today but didn't fully dig into why.

## Proof of work
- [x] Created a 4x4 array (values 1–16), printed shape and dtype
- [x] Sliced out a column and the last row
- [x] Used a boolean mask to filter values above the mean
- [x] Added a 1D array to a 2D array via broadcasting and manually checked the result
