# Day 2 — What I Learned: Python Fundamentals for Data Work

Today was a refresher on core Python, but with an eye toward how it actually gets used in data science rather than just as general programming syntax.

## What I did
I went back through Python's basic data types (numbers, strings, booleans, lists, tuples, dicts, sets) and practiced control flow with a couple of small loops. Then I wrote some functions with proper docstrings, rewrote a loop as a list comprehension, and built a tiny class to represent a data record.

## What clicked for me
- I already knew loops and if/else, but what's new is realizing how much *less* I'll actually use plain loops going forward — vectorized operations in NumPy and Pandas (coming up Days 3–4) replace most of what I'd normally write a `for` loop for. So today felt more like "know this well enough to read it" than "this is my daily tool."
- List comprehensions finally feel natural instead of like a syntax trick. Writing `[x**2 for x in range(10)]` instead of a 3-line loop is genuinely easier to read once I got used to reading it right-to-left (what am I building, then where's it coming from, then any filter).
- Classes clicked in a new way today too — I'm not going to be writing tons of my own classes, but understanding `__init__` and methods matters because things like Scikit-learn models are just objects built this way. Recognizing `self.x` and `self.y` as "this is just data attached to an object" made a Scikit-learn example I looked at afterward much less intimidating.

## Code I want to remember
```python
def normalize(value, min_val, max_val):
    """Scale a value to the 0-1 range."""
    return (value - min_val) / (max_val - min_val)

class DataPoint:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def distance_from_origin(self):
        return (self.x**2 + self.y**2) ** 0.5
```

## Still a bit fuzzy
- When exactly a list comprehension becomes "too clever" and hurts readability instead of helping it — I want to develop better judgment here rather than using one everywhere out of habit.
- I only wrote a class with one method today. I'd like to see a slightly more complex example (maybe one with inheritance) to make sure the concept actually sticks.

## Proof of work
- [x] Function that returns mean/min/max of a list as a dict
- [x] For-loop rewritten as a list comprehension
- [x] Small class with two attributes and one method
- [x] Notebook cells documented with Markdown explanations
