# Day 5 — What I Learned: Visualizing Data with Matplotlib

Today wrapped up Week 1 by putting everything together — NumPy, Pandas, and now Matplotlib — into one notebook.

## What I did
I practiced the four core plot types (line, scatter, bar, histogram) and learned how to combine multiple charts into one figure using subplots. Then I built a mini-notebook that loaded a dataset, cleaned it with Pandas, computed a stat with NumPy, and visualized the results with at least three labeled plots.

## What clicked for me
- The line "an unlabeled chart is a puzzle, not a finding" is something I want to actually hold onto — I made a plot early in the day without axis labels and immediately couldn't tell someone else what it meant, even though I'd just made it myself. Labels aren't decoration, they're what makes a chart useful to anyone besides me.
- Picking the *right* plot type finally feels intentional instead of arbitrary. A histogram answers "what's the spread of this one variable," a scatter plot answers "how do these two variables relate," a bar chart is for comparing categories. Having that mental checklist made choosing a plot type way faster.
- Subplots were the most satisfying part of the day — being able to put a histogram and a scatter plot side by side in one figure made it much easier to compare two things at once instead of scrolling between separate outputs.
- Putting the full pipeline together (load → clean → compute → visualize) in one notebook made the whole week click into place. Each day this week was really just one stage of this same pipeline, and today is where I saw them connect for the first time.

## Code I want to remember
```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 2, figsize=(10, 4))
axes[0].hist(df["age"])
axes[0].set_title("Age distribution")
axes[1].scatter(df["age"], df["income"])
axes[1].set_title("Age vs. Income")
plt.tight_layout()
plt.show()
```

## Still a bit fuzzy
- Styling plots beyond the basics (colors, themes, making things presentation-ready) — everything I made today is functional but not something I'd put in front of a client yet.
- I want more practice deciding *how many* plots are actually useful for a dataset versus just generating charts for the sake of it.

## Proof of work
- [x] Loaded and cleaned a dataset with Pandas
- [x] Computed at least one derived stat with NumPy
- [x] Produced three labeled plots, including a histogram and a scatter plot
- [x] Wrote Markdown explaining what each plot reveals
- [x] Committed the finished notebook and `requirements.txt` to GitHub

