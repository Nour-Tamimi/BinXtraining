## Day 3 - Linear Algebra for ML 

## Vector
A list of numbers describing one sample.

## Matrix
Many vectors stacked = a full dataset. Shape = (rows, columns) = (samples, features).

## Dot Product
Multiply matching positions, then sum. Gives one number.

This is how a model predicts: `prediction = dot(features, weights) + bias`

## Bias
A flat number added after the dot product. Lets the prediction start somewhere other than zero, even if all features are 0. Same idea as `b` in `y = mx + b`.

## Matrix Multiplication
Does the dot product for every sample at once.
```
(m x n) @ (n x p) -> (m x p)
```
Inner dimensions must match. The matching dimension disappears (gets summed away).

## `*` vs `@`
- `*` (element-wise): shapes must match exactly (or one is a scalar). Multiplies position by position, no summing.
- `@` / `np.dot` =: only the inner dimensions must match. Multiplies then sums, collapsing the shared dimension.

## Shape `(5,)` vs `(5,1)` vs `(1,5)`
- `(5,)` = flat vector, no rows/columns concept
- `(5,1)` = column vector (matrix), 5 rows, 1 column
- `(1,5)` = row vector (matrix), 1 row, 5 columns
- Same numbers, different shape — matters for how operations behave.

## Norm
Measures the "size" (length) of a vector.
- L2 norm: straight-line length -> `sqrt(sum of squares)`
- L1 norm: sum of absolute values -> "block by block" distance
Used for: regularization (keeping weights small), measuring distance between points, normalizing data.

## Basis
The smallest set of building-block directions needed to construct any vector. Example: `[1,0]` (right) and `[0,1]` (up) can build any 2D point. In ML, your chosen features are basically your basis directions.

## Big Picture
- Vector = one sample
- Matrix = many samples
- Dot product = one prediction from one sample's features + weights
- Matrix multiplication = one prediction per sample, for the whole dataset
- Bias = flat shift added to every prediction
- Norm = size of a vector
- Basis = the building-block directions vectors are made from
