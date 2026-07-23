# Day 1 — What I Learned: Setting Up My Dev Environment

Today was all about getting my machine ready before touching any real data science work.

## What I did
I created my first virtual environment for this internship, so all the packages I install for this program stay isolated from anything else on my machine. Inside it I installed numpy, pandas, matplotlib, and jupyter, then saved those exact versions to a `requirements.txt` file so I (or anyone else) can rebuild the same setup later.

I also opened Jupyter Notebook for the first time in this context and got comfortable switching between code cells and Markdown cells — writing an explanation in Markdown, then running the code right below it. Finally, I set up a Git repo for my work and made my first commit.

## What clicked for me
- I used to just install packages globally, but now I get *why* that's a bad habit — one project's version of a library can silently break another project. A virtual environment per project avoids that entirely.
- `pip freeze > requirements.txt` is basically a snapshot of my environment. If my notebook works today but breaks in six months, this file is how I (or someone else) can recreate the exact setup that worked.
- A notebook isn't just "code with some notes." Treating it like a short report — explain in Markdown, then show the code, then show the output — makes it something someone else can actually follow later. I want to get in the habit of writing the Markdown *before* I write the code, not after.

## Commands I'll reuse
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install numpy pandas matplotlib jupyter
pip freeze > requirements.txt
```

## Still a bit fuzzy
- venv vs. conda — I used venv today, but I want to try conda on a future project to see what the actual difference feels like in practice.
- Git workflow beyond a single commit (branches, meaningful commit messages) — I did the bare minimum today and want to get more disciplined about this as the notebooks pile up.

## Proof of work
- [x] Environment created and packages installed, versions confirmed with a code cell
- [x] `requirements.txt` committed
- [x] First notebook (code + Markdown) pushed to GitHub
