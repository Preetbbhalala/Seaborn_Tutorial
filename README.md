<p align="center">
  <img src="Seaborn.png" alt="seaborn logo" width="480">
</p>

<h1 align="center">Seaborn Tutorial</h1>

<p align="center">
  Notes and examples for learning <b>Seaborn</b>, the Python library for statistical data visualization built on top of Matplotlib.
</p>

<p align="center">
  <a href="https://colab.research.google.com/github/Preetbbhalala/seaborn_tutorial/blob/main/seaborn_tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a>
  <a href="https://www.python.org/downloads/release/python-390/"><img src="https://img.shields.io/badge/python-3.9%2B-3776AB?logo=python&logoColor=white" alt="Python 3.9+"></a>
  <a href="https://seaborn.pydata.org/whatsnew/v0.13.2.html"><img src="https://img.shields.io/badge/seaborn-0.13-4C72B0" alt="seaborn 0.13"></a>
  <img src="https://img.shields.io/badge/jupyter-notebook-F37626?logo=jupyter&logoColor=white" alt="Jupyter notebook">
</p>

## About

Seaborn turns a pandas DataFrame into a clear, good-looking statistical plot, usually in one or two lines of code. It takes care of grouping, colors, legends and the statistics (averages, confidence intervals, density curves, regression lines). Because it is built on Matplotlib, every plot can still be fine-tuned with Matplotlib.

This repository holds my notes and code from a seven-lesson Seaborn course: introduction, creating plots, relational, categorical and distribution plots, heatmaps, and best practices for data visualization.

## What's in this repo

| File | What it is |
|---|---|
| [`seaborn_tutorial.ipynb`](seaborn_tutorial.ipynb) | Jupyter notebook with my hands-on examples from the course |
| `README.md` | The full course notes: explanations, runnable code for every plot type, best practices and a cheat sheet |
| `Seaborn.png` | The Seaborn logo used above |

What the notebook covers, lesson by lesson:

| Lesson | Example in the notebook |
|---|---|
| Introduction | Imports seaborn and Matplotlib, applies `sns.set_theme()` and loads the `tips` dataset |
| Creating plots | `relplot` of tip vs. total bill with one panel per `time`, colored and shaped by `smoker`, sized by party `size`; a line `relplot` of y = x² |
| Relational plots | `scatterplot` of tip vs. total bill colored by `time`; `lineplot` of airline passengers by year |
| Categorical plots | `barplot` of the average bill per day, split by `sex`; `boxplot` of bills per day |
| Distribution plots | `histplot` of penguin body mass with 30 bins |
| Heatmaps | Monthly airline passengers pivoted into a month × year table and drawn as an annotated heatmap |
| Best practices | A fully labelled line chart (title, axis labels with units, legend title) built with `fig, ax = plt.subplots()` |

## Getting started

**Run it online:** click **Open in Colab** at the top of this page. There's nothing to install.

**Run it locally:**

```bash
git clone https://github.com/Preetbbhalala/seaborn_tutorial.git
cd seaborn_tutorial
pip install seaborn "pandas<3" jupyter
jupyter notebook seaborn_tutorial.ipynb
```

The example datasets (`tips`, `flights`, `penguins`, …) are downloaded by `sns.load_dataset()` the first time they're used, so the first run needs an internet connection.

> [!IMPORTANT]
> **Why `pandas<3`?** seaborn 0.13.2, the latest release as of October 2026, has a bug with pandas 3. Plots that split the data by a categorical column *and* another variable at the same time can draw one group's data under another group's label, with no error. Examples are `hue=` together with `col=`, or a split violin plot. pandas 2.x works correctly.

## Course notes

The notes below explain each lesson in plain words, with code you can run. Run the [Setup](#0-setup) block first; after that, each block draws the plot it describes. They're written for seaborn 0.13. Older tutorials use some names that have since changed, and the [Cheat Sheet](#cheat-sheet) maps old names to new ones.

| # | Lesson | Covers |
|---|---|---|
| 0 | [Setup](#0-setup) | Imports and example datasets |
| 1 | [Introduction to Seaborn](#1-introduction-to-seaborn) | What Seaborn is, Seaborn vs. Matplotlib, tidy data, themes and palettes |
| 2 | [Creating Plots with Seaborn](#2-creating-plots-with-seaborn) | The one pattern behind every plot, figure-level vs. axes-level, customizing, saving |
| 3 | [Relational Plots](#3-relational-plots-in-seaborn) | Scatter and line plots, `hue` / `size` / `style`, panels, confidence bands, trend lines |
| 4 | [Categorical Plots](#4-categorical-plots-in-seaborn) | Strip, swarm, box, violin, boxen, bar, point and count plots |
| 5 | [Distribution Plots](#5-distribution-plots-in-seaborn) | Histograms, KDE, ECDF, 2-D distributions, joint and pair plots |
| 6 | [Relational Plots — Heatmaps](#6-relational-plots-in-seaborn--heatmaps) | Pivoting data into a matrix, annotated heatmaps, correlation heatmaps |
| 7 | [Best Practices for Data Visualization](#7-best-practices-for-data-visualization) | Choosing the chart, honest scales, color, clutter, before/after fixes, checklist |
| ★ | [Cheat Sheet](#cheat-sheet) | Which function to use, common arguments, old → new names |

## 0. Setup

Every example below starts from these imports and example datasets, so run this block first:

```python
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme()   # seaborn's default look: grey background grid + the "deep" color palette

print("seaborn   ", sns.__version__)
print("pandas    ", pd.__version__)
print("matplotlib", mpl.__version__)
```

Output:

```text
seaborn    0.13.2
pandas     2.3.3
matplotlib 3.11.2
```

```python
# Example datasets that ship with seaborn (downloaded the first time, then cached)
tips     = sns.load_dataset("tips")      # restaurant bills and tips
fmri     = sns.load_dataset("fmri")      # brain-scan signal over time, many subjects
flights  = sns.load_dataset("flights")   # monthly airline passengers, 1949–1960
titanic  = sns.load_dataset("titanic")   # Titanic passengers and who survived
penguins = sns.load_dataset("penguins")  # body measurements of three penguin species
diamonds = sns.load_dataset("diamonds")  # ~54,000 diamonds: carat, cut, price, ...

for name, df in {"tips": tips, "fmri": fmri, "flights": flights,
                 "titanic": titanic, "penguins": penguins, "diamonds": diamonds}.items():
    print(f"{name:9} {df.shape[0]:>6,} rows × {df.shape[1]:>2} columns")
```

Output:

```text
tips         244 rows ×  7 columns
fmri       1,064 rows ×  5 columns
flights      144 rows ×  3 columns
titanic      891 rows × 15 columns
penguins     344 rows ×  7 columns
diamonds  53,940 rows × 10 columns
```

## 1. Introduction to Seaborn

**Seaborn** is a Python library for **statistical data visualization**. It is built on top of **Matplotlib** and designed to work directly with **pandas DataFrames**.

The big idea: with Matplotlib you tell the computer *how* to draw (loop over the groups, pick colors, add a legend). With Seaborn you describe *what* to show — which column goes on x, which on y, which column decides the color — and Seaborn does the drawing **and the statistics** (averages, confidence intervals, density curves, regression lines).

#### Seaborn vs. Matplotlib

| | Matplotlib | Seaborn |
|---|---|---|
| Level | Low-level: you draw each element | High-level: you describe the plot with column names |
| Input | Lists / arrays | A DataFrame + column names: `data=df, x="col"` |
| Split the data by a category | Write a loop, one group at a time | One argument: `hue="col"` |
| Statistics | Compute them yourself | Built in: means, confidence intervals, KDE, regression |
| Default look | Plain | Polished themes and color palettes |
| Legend and axis labels | Added by hand | Created automatically from the data |

Seaborn does **not** replace Matplotlib. Every Seaborn plot *is* a Matplotlib figure, so everything you know from Matplotlib (`ax.set_title`, `ax.set_xlim`, `fig.savefig`, …) still works for fine-tuning.

Here is the same chart made both ways:

```python
# Tip vs. total bill, colored by lunch/dinner — once with Matplotlib, once with Seaborn
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4), sharey=True)

# Matplotlib: loop over the groups, color them, label them, build the legend yourself
for time, group in tips.groupby("time", observed=True):
    ax1.scatter(group["total_bill"], group["tip"], label=time, alpha=0.8)
ax1.set(title="Matplotlib: a loop, labels and a legend by hand", xlabel="total_bill", ylabel="tip")
ax1.legend(title="time")

# Seaborn: one line — grouping, colors, axis labels and legend come from the data
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="time", ax=ax2)
ax2.set_title("Seaborn: one line")

plt.show()
```

Same result — but Seaborn found the groups, picked the colors and built the legend and axis labels from the DataFrame itself.

### 1.1 Built-in example datasets

`sns.load_dataset("name")` downloads a small practice dataset from Seaborn's online data repository (internet is needed the first time) and returns a pandas DataFrame. All available names:

```python
sns.get_dataset_names()
```

Output:

```text
['anagrams',
 'anscombe',
 'attention',
 'brain_networks',
 'car_crashes',
 'diamonds',
 'dots',
 'dowjones',
 'exercise',
 'flights',
 'fmri',
 'geyser',
 'glue',
 'healthexp',
 'iris',
 'mpg',
 'penguins',
 'planets',
 'seaice',
 'taxis',
 'tips',
 'titanic']
```

The **tips** dataset has one row per restaurant bill:

```python
tips.head()
```

Output:

```text
   total_bill   tip     sex smoker  day    time  size
0       16.99  1.01  Female     No  Sun  Dinner     2
1       10.34  1.66    Male     No  Sun  Dinner     3
2       21.01  3.50    Male     No  Sun  Dinner     3
3       23.68  3.31    Male     No  Sun  Dinner     2
4       24.59  3.61  Female     No  Sun  Dinner     4
```

| Column | Meaning | Type |
|---|---|---|
| `total_bill` | bill amount in dollars | numeric |
| `tip` | tip in dollars | numeric |
| `sex` | sex of the person paying | categorical |
| `smoker` | were there smokers in the party? | categorical |
| `day` | day of the week (Thur–Sun) | categorical |
| `time` | Lunch or Dinner | categorical |
| `size` | number of people at the table | numeric (whole numbers) |

Seaborn looks at each column's **type** and treats it differently: **numeric** columns get continuous axes and color *gradients*; **categorical** columns get separate groups and *distinct* colors. Check the types with `df.dtypes`:

```python
tips.dtypes
```

Output:

```text
total_bill     float64
tip            float64
sex           category
smoker        category
day           category
time          category
size             int64
dtype: object
```

### 1.2 Tidy ("long-form") data — the shape Seaborn likes

Seaborn works best with **tidy** data:
1. every **column** is one variable
2. every **row** is one observation

Then any column can be sent to x, y, color, size or its own panel just by naming it. `flights` is tidy — one row per year + month:

```python
flights.head()
```

Output:

```text
   year month  passengers
0  1949   Jan         112
1  1949   Feb         118
2  1949   Mar         132
3  1949   Apr         129
4  1949   May         121
```

The same numbers in **wide** form — one row per year, one column per month. It's easier for a human to scan, but "month" is no longer a column, so you couldn't write `hue="month"`:

```python
flights_wide = flights.pivot(index="year", columns="month", values="passengers")
flights_wide.head()
```

Output:

```text
month  Jan  Feb  Mar  Apr  May  Jun  Jul  Aug  Sep  Oct  Nov  Dec
year                                                             
1949   112  118  132  129  121  135  148  148  136  119  104  118
1950   115  126  141  135  125  149  170  170  158  133  114  140
1951   145  150  178  163  172  178  199  199  184  162  146  166
1952   171  180  193  181  183  218  230  242  209  191  172  194
1953   196  196  236  235  229  243  264  272  237  211  180  201
```

Converting between the two shapes:
- **long → wide:** `df.pivot(...)` or `df.pivot_table(...)` — you'll need this for heatmaps in Lesson 6
- **wide → long:** `df.melt(...)`

```python
flights_long = flights_wide.reset_index().melt(id_vars="year", value_name="passengers")
flights_long.head()
```

Output:

```text
   year month  passengers
0  1949   Jan         112
1  1950   Jan         115
2  1951   Jan         145
3  1952   Jan         171
4  1953   Jan         196
```

### 1.3 The look: themes, styles, contexts and palettes

`sns.set_theme()` sets the look of **every** plot that follows:

| Argument | Options | Controls |
|---|---|---|
| `style` | `"darkgrid"` (default), `"whitegrid"`, `"dark"`, `"white"`, `"ticks"` | background and grid lines |
| `context` | `"paper"`, `"notebook"` (default), `"talk"`, `"poster"` | size of text and lines, small → large |
| `palette` | `"deep"` (default), `"muted"`, `"pastel"`, `"bright"`, `"dark"`, `"colorblind"`, … | colors used for groups |
| `font_scale` | e.g. `1.2` | multiplies every font size |

Example: `sns.set_theme(style="whitegrid", context="talk", palette="colorblind")`.

To change the look of **one** plot only, use a `with` block: `sns.axes_style(...)` for the style, `sns.plotting_context(...)` for the size. All five styles side by side:

```python
x = np.linspace(0, 10, 100)

fig = plt.figure(figsize=(16, 3))
for i, style in enumerate(["darkgrid", "whitegrid", "dark", "white", "ticks"], start=1):
    with sns.axes_style(style):              # this style applies only inside the block
        ax = fig.add_subplot(1, 5, i)
        for shift in range(3):
            sns.lineplot(x=x, y=np.sin(x + shift), ax=ax)
        ax.set_title(f'style="{style}"')
fig.tight_layout()
plt.show()
```

`sns.despine()` removes the top and right border lines (the "spines") — the usual finishing touch for the `"white"` and `"ticks"` styles.

#### Color palettes

There are three kinds of palette. Picking the right **kind** for your data matters more than picking pretty colors:

| Kind | Use it for | Examples |
|---|---|---|
| **Qualitative** | groups with no order (species, day, sex) | `"deep"`, `"colorblind"`, `"muted"`, `"Set2"`, `"tab10"` |
| **Sequential** | ordered amounts, low → high | `"rocket"`, `"mako"`, `"crest"`, `"flare"`, `"viridis"`, `"Blues"` |
| **Diverging** | values above / below a meaningful middle, e.g. correlations around 0 | `"vlag"`, `"icefire"`, `"coolwarm"`, `"RdBu"` |

`sns.color_palette(name)` returns a palette; in a notebook it shows as color swatches. Add `_r` to reverse a palette (`"rocket_r"`), or pass `as_cmap=True` to get a continuous colormap.

```python
for kind, name in [("qualitative", "deep"), ("qualitative", "colorblind"),
                   ("sequential", "rocket"), ("sequential", "crest"),
                   ("diverging", "vlag")]:
    print(f"{name}  ({kind})")
    display(sns.color_palette(name))
```

> [!TIP]
> **Recap — Lesson 1**
> - Seaborn = high-level, statistics-aware plotting built on Matplotlib, designed for DataFrames.
> - Pass `data=` plus **column names**; Seaborn handles grouping, colors, legends and the statistics.
> - Tidy (long-form) data lets any column become x, y, `hue`, `size`, `style`, `col` or `row`. Use `melt` / `pivot` to reshape.
> - `sns.set_theme()` controls style, context (scale) and palette. Choose a qualitative, sequential or diverging palette based on the data.

## 2. Creating Plots with Seaborn

### 2.1 One pattern for every plot

Nearly every Seaborn function is called the same way:

```python
sns.plot_function(data=df, x="column_a", y="column_b", hue="column_c")
```

| Argument | What it means |
|---|---|
| `data` | the DataFrame |
| `x`, `y` | which **columns** go on each axis (names as strings) |
| `hue` | a column that controls **color** — the data is split into colored groups |
| `ax` | which subplot to draw on (axes-level functions only — see 2.3) |

Learn this once and you can use all of Seaborn's ~20 plot functions. Three different plot types, identical calls:

```python
fig, axes = plt.subplots(1, 3, figsize=(16, 4))

sns.histplot(data=tips, x="total_bill", ax=axes[0])               # one numeric column
sns.boxplot(data=tips, x="day", y="total_bill", ax=axes[1])       # categorical vs. numeric
sns.scatterplot(data=tips, x="total_bill", y="tip", ax=axes[2])   # numeric vs. numeric

axes[0].set_title("sns.histplot")
axes[1].set_title("sns.boxplot")
axes[2].set_title("sns.scatterplot")
fig.tight_layout()
plt.show()
```

### 2.2 The plot families

Seaborn's functions are grouped into families. Each of the three main families has one **figure-level** function and several **axes-level** functions that do the actual drawing. The figure-level function picks one of them with `kind=`:

| Family | Figure-level | Axes-level functions | `kind=` values | Shows |
|---|---|---|---|---|
| Relational | `relplot` | `scatterplot`, `lineplot` | `"scatter"`, `"line"` | how two numeric variables relate |
| Distribution | `displot` | `histplot`, `kdeplot`, `ecdfplot`, `rugplot` | `"hist"`, `"kde"`, `"ecdf"` | how values are spread |
| Categorical | `catplot` | `stripplot`, `swarmplot`, `boxplot`, `violinplot`, `boxenplot`, `pointplot`, `barplot`, `countplot` | `"strip"`, `"swarm"`, `"box"`, `"violin"`, `"boxen"`, `"point"`, `"bar"`, `"count"` | a number compared across groups |

Other useful functions:

| Family | Functions | Shows |
|---|---|---|
| Regression | `lmplot` (figure-level), `regplot`, `residplot` | a fitted trend line through the data |
| Matrix | `heatmap`, `clustermap` | a table of values as colored cells |
| Multi-plot grids | `pairplot`, `jointplot` | many relationships at once |

### 2.3 Figure-level vs. axes-level — the most important idea in Seaborn

| | Axes-level (`scatterplot`, `histplot`, `boxplot`, …) | Figure-level (`relplot`, `displot`, `catplot`, …) |
|---|---|---|
| Draws on | one Matplotlib `Axes` (a single plot area) | a whole new figure that it creates itself |
| Returns | a Matplotlib `Axes` | a Seaborn `FacetGrid` |
| Can go inside your own subplots? | **Yes** — pass `ax=` | **No** — it always makes its own figure |
| Size | `fig, ax = plt.subplots(figsize=(w, h))` | `height=` (inches per panel) and `aspect=` (width = height × aspect) |
| One panel per group ("small multiples") | No | **Yes** — `col=` and `row=` |
| Legend | inside the plot | outside, on the right |
| Titles and labels | `ax.set_title(...)`, `ax.set(xlabel=...)` | `g.set_titles(...)`, `g.set_axis_labels(...)`, `g.figure.suptitle(...)` |

**Rule of thumb:** use figure-level functions for quick exploration and small multiples; use axes-level functions when you build your own layout of several different plots.

```python
# Axes-level: you create the figure and axes, Seaborn draws into them
fig, ax = plt.subplots(figsize=(7, 4))
result = sns.histplot(data=tips, x="total_bill", hue="time", ax=ax)
ax.set(title="Axes-level: sns.histplot", xlabel="Total bill ($)", ylabel="Number of bills")

print("histplot returned:", type(result).__name__)
plt.show()
```

Output:

```text
histplot returned: Axes
```

```python
# Figure-level: Seaborn creates the figure; col= makes one panel per value of 'sex'
g = sns.displot(data=tips, x="total_bill", hue="time", col="sex", height=3.5, aspect=1.2)
g.set_axis_labels("Total bill ($)", "Number of bills")
g.set_titles("Payer: {col_name}")              # {col_name} is filled in for each panel
g.figure.suptitle("Figure-level: sns.displot with col='sex'", y=1.05)

print("displot returned:", type(g).__name__)
plt.show()
```

Output:

```text
displot returned: FacetGrid
```

### 2.4 Customizing a plot

Where to make the most common changes:

| To change… | Axes-level plot (`ax`) | Figure-level plot (`g`) |
|---|---|---|
| Title | `ax.set_title("…")` | `g.figure.suptitle("…")` for the figure, `g.set_titles("{col_name}")` for panels |
| Axis labels | `ax.set(xlabel="…", ylabel="…")` | `g.set_axis_labels("…", "…")` |
| Axis limits | `ax.set(xlim=(0, 60))` | `g.set(xlim=(0, 60))` |
| Rotate tick labels | `ax.tick_params(axis="x", rotation=45)` | `g.tick_params(axis="x", rotation=45)` |
| Figure size | `plt.subplots(figsize=(8, 4))` | `height=4, aspect=2` in the call |
| Move the legend | `sns.move_legend(ax, "upper left", bbox_to_anchor=(1, 1))` | `sns.move_legend(g, "upper left", bbox_to_anchor=(1, 1))` |
| Hide the legend | `legend=False` in the call | `legend=False` in the call |

Inside the plot call itself:
- `color="seagreen"` — one fixed color for everything (no grouping)
- `palette="colorblind"` — the colors for the `hue` groups: a palette name, a list of colors, or a dict like `{"Lunch": "orange", "Dinner": "navy"}`
- `order=[...]`, `hue_order=[...]` — the order of the categories
- most Matplotlib styling keywords pass straight through: `alpha`, `s` (marker size), `edgecolor`, `linewidth`, `marker`, …

```python
fig, ax = plt.subplots(figsize=(8, 4.5))
sns.scatterplot(
    data=tips, x="total_bill", y="tip",
    hue="day", hue_order=["Thur", "Fri", "Sat", "Sun"],   # fixed category order
    palette="colorblind",                                  # colorblind-safe colors
    s=60, alpha=0.8, edgecolor="white",                    # Matplotlib styling passes through
    ax=ax,
)
ax.set(title="Bigger bills get bigger tips", xlabel="Total bill ($)", ylabel="Tip ($)", xlim=(0, 55))
sns.move_legend(ax, "upper left", bbox_to_anchor=(1, 1), title="Day")
plt.show()
```

### 2.5 Several plots in one figure

Because axes-level functions accept `ax=`, you can arrange any of them in a Matplotlib grid with `plt.subplots(rows, cols)`:

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 8))

sns.histplot(data=tips, x="tip", kde=True, ax=axes[0, 0])
sns.boxplot(data=tips, x="time", y="tip", ax=axes[0, 1])
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="smoker", ax=axes[1, 0])
sns.countplot(data=tips, x="day", ax=axes[1, 1])

axes[0, 0].set_title("How big are tips?")
axes[0, 1].set_title("Tips at lunch vs. dinner")
axes[1, 0].set_title("Tip vs. bill")
axes[1, 1].set_title("Bills per day")

fig.suptitle("Four views of the tips data", fontsize=15)
fig.tight_layout()
plt.show()
```

### 2.6 Saving a plot

```python
fig.savefig("my_plot.png", dpi=300, bbox_inches="tight")   # axes-level: save the Matplotlib figure
g.savefig("my_grid.png", dpi=300, bbox_inches="tight")     # figure-level: the FacetGrid has its own savefig
```

- `dpi=300` → sharp enough for print and slides
- `bbox_inches="tight"` → trims extra white space and keeps a legend that sits outside the plot
- `.svg` or `.pdf` instead of `.png` → vector graphics that stay sharp at any size
- Save in the **same cell / before `plt.show()`**. Once a figure has been shown it is closed, and `plt.savefig()` afterwards saves an empty image.

> [!TIP]
> **Recap — Lesson 2**
> - Every function: `sns.func(data=df, x=..., y=..., hue=...)`.
> - Axes-level functions draw on one `ax` (use `ax=` to place them in subplots); figure-level functions (`relplot`, `displot`, `catplot`) build their own figure, size with `height`/`aspect`, and make panels with `col`/`row`.
> - Customize with `ax.set(...)` / `g.set_axis_labels(...)`, `palette`, `hue_order`, `sns.move_legend`, and Matplotlib keywords.
> - Save with `savefig(..., dpi=300, bbox_inches="tight")`.

## 3. Relational Plots in Seaborn

Relational plots answer: **how does one numeric variable change with another?**

| Function | Draws | Use when |
|---|---|---|
| `scatterplot()` — or `relplot(kind="scatter")` | one dot per row | both variables are numeric; look for patterns, clusters and outliers |
| `lineplot()` — or `relplot(kind="line")` | a line, plus a shaded band | x has a natural order (time, dose, distance) and you care about the trend |

`relplot()` is the figure-level function (it can make panels with `col`/`row`); `scatterplot()` and `lineplot()` are its axes-level versions.

### 3.1 Scatter plots

```python
sns.relplot(data=tips, x="total_bill", y="tip", height=4, aspect=1.5)
plt.show()
```

Each dot is one bill. The cloud rises to the right — bigger bills tend to get bigger tips (a **positive relationship**) — and it fans out as bills get bigger.

#### More variables with `hue`, `size` and `style`

Position shows two variables. Seaborn's **semantic** arguments add more:

| Argument | Shows a column as… | Works best with |
|---|---|---|
| `hue` | color | categorical (distinct colors) **or** numeric (a color gradient) |
| `size` | dot size / line width | numeric |
| `style` | marker shape / line dash pattern | categorical with few values |

Mapping the **same** column to both `hue` and `style` is a good habit: the groups stay distinguishable in black-and-white print and for colorblind readers.

```python
sns.relplot(data=tips, x="total_bill", y="tip",
            hue="smoker", style="smoker",       # same column → color AND marker shape
            height=4, aspect=1.5)
plt.show()
```

```python
# A numeric hue switches automatically to a sequential color gradient
sns.relplot(data=tips, x="total_bill", y="tip",
            hue="size", size="size", sizes=(20, 200),   # sizes = (smallest, largest) marker area
            palette="crest", height=4, aspect=1.5)
plt.show()
```

> **Don't overdo it.** `hue`, `size` *and* `style` on three *different* columns make a plot that's hard to read. Two extra variables is usually the limit — use panels for the rest.

#### Small multiples with `col` and `row`

Instead of piling everything onto one plot, split it into a grid of panels that share the same axes. Only the figure-level `relplot()` can do this:

```python
sns.relplot(data=tips, x="total_bill", y="tip",
            hue="smoker", style="smoker",
            col="time",       # one column of panels per value of 'time'
            row="sex",        # one row of panels per value of 'sex'
            height=3.2, aspect=1.3)
plt.show()
```

With a column that has many values, use `col="day", col_wrap=2` to wrap the panels onto several rows.

### 3.2 Line plots

Use a line plot when x has an **order** — usually time. The `fmri` dataset holds a brain-scan signal measured at 19 time points for 14 people, for two kinds of event and two brain regions:

```python
fmri.head()
```

Output:

```text
  subject  timepoint event    region    signal
0     s13         18  stim  parietal -0.017552
1      s5         14  stim  parietal -0.080883
2     s12         18  stim  parietal -0.081033
3     s11         18  stim  parietal -0.046134
4     s10         18  stim  parietal -0.037970
```

#### Automatic aggregation and the confidence band

`fmri` has **many** `signal` values for each `timepoint` (one per subject, event and region). Instead of a tangle of lines, `lineplot` **aggregates**:
- the line is the **mean** of all y values at each x
- the shaded band is the **95% confidence interval** of that mean (estimated by bootstrapping) — how sure we are about the average

```python
sns.relplot(data=fmri, kind="line", x="timepoint", y="signal", height=4, aspect=1.6)
plt.show()
```

Two arguments control this:

| Argument | Examples | Effect |
|---|---|---|
| `estimator` | `"mean"` (default), `"median"`, `"sum"`, `None` | how y values at the same x are combined; `None` = draw the raw data |
| `errorbar` | `("ci", 95)` (default), `"sd"`, `"se"`, `("pi", 50)`, `None` | what the band shows: confidence interval, standard deviation, standard error, percentile interval, or nothing |

> Older tutorials use `ci=95` or `ci="sd"`. That argument was replaced by `errorbar=` in seaborn 0.12.

```python
fig, axes = plt.subplots(1, 3, figsize=(16, 4), sharey=True)

sns.lineplot(data=fmri, x="timepoint", y="signal", errorbar=("ci", 95), ax=axes[0])
axes[0].set_title("Mean + 95% confidence interval (default)")

sns.lineplot(data=fmri, x="timepoint", y="signal", errorbar="sd", ax=axes[1])
axes[1].set_title("Mean + 1 standard deviation")

one_condition = fmri.query("region == 'frontal' and event == 'stim'")
sns.lineplot(data=one_condition, x="timepoint", y="signal",
             units="subject", estimator=None,          # one line per subject, no averaging
             linewidth=1, alpha=0.6, ax=axes[2])
axes[2].set_title("No aggregation: one line per subject")

plt.show()
```

The confidence interval (left) is narrow because it describes uncertainty about the **average**; the standard deviation (middle) is wider because it describes how much **individual measurements** vary. Pick the one that matches your question.

#### Groups in line plots

`hue` draws one colored line per group, `style` changes the dash pattern, and `markers=True` puts a marker on each data point:

```python
sns.relplot(data=fmri, kind="line", x="timepoint", y="signal",
            hue="event", style="event", markers=True, dashes=False,
            col="region",                 # one panel per brain region
            height=3.8, aspect=1.2)
plt.show()
```

#### A different estimator: yearly totals

`flights` has 12 monthly values per year. `estimator="sum"` turns the line into **yearly totals** instead of the monthly average, and `errorbar=None` removes the band (it makes no sense for a total):

```python
fig, ax = plt.subplots(figsize=(8, 4))
sns.lineplot(data=flights, x="year", y="passengers", estimator="sum", errorbar=None,
             marker="o", ax=ax)
ax.set(title="Airline passengers per year", xlabel="Year", ylabel="Passengers (thousands)")
plt.show()
```

### 3.3 Adding a trend line: `regplot` and `lmplot`

To see the overall direction of a relationship, fit a regression line. `regplot` (axes-level) and `lmplot` (figure-level, supports `hue`, `col`, `row`) draw the scatter, a **straight-line fit** and a 95% confidence band around the fit:

```python
sns.lmplot(data=tips, x="total_bill", y="tip", hue="smoker", height=4, aspect=1.5)
plt.show()
```

Useful options: `order=2` (curved, polynomial fit), `lowess=True` (smooth curve with no straight-line assumption) and `logistic=True` (for a 0/1 y) — both need the `statsmodels` package — plus `scatter_kws={"alpha": 0.4}` to style the dots and `ci=None` to remove the band. (Regression plots still use `ci=`, not `errorbar=`.)

> [!TIP]
> **Recap — Lesson 3**
> - `relplot` (figure-level) → `scatterplot` / `lineplot` (axes-level).
> - `hue` → color, `size` → size, `style` → marker / dash. Mapping one column to both `hue` and `style` keeps groups readable without color.
> - `col`, `row`, `col_wrap` → small multiples with shared axes.
> - `lineplot` aggregates repeated x values: mean + 95% CI by default. Change it with `estimator=` and `errorbar=`; use `units=` + `estimator=None` to draw every individual.
> - `lmplot` / `regplot` add a fitted trend line.

## 4. Categorical Plots in Seaborn

Categorical plots compare a **number** across **groups** (days, classes, species). There are three families, all available through `sns.catplot(kind=...)`:

| Family | Functions (`kind=`) | Shows | Use when |
|---|---|---|---|
| **Categorical scatter** | `stripplot` (`"strip"`), `swarmplot` (`"swarm"`) | every single observation | small or medium data — you want to see all the points |
| **Distribution** | `boxplot` (`"box"`), `violinplot` (`"violin"`), `boxenplot` (`"boxen"`) | the spread of each group | comparing distributions; boxen for large datasets |
| **Estimate** | `barplot` (`"bar"`), `pointplot` (`"point"`), `countplot` (`"count"`) | one number per group (an average or a count) + its uncertainty | comparing averages or counts |

### 4.1 Categorical scatter: strip and swarm

```python
fig, axes = plt.subplots(1, 2, figsize=(14, 4.2), sharey=True)

sns.stripplot(data=tips, x="day", y="total_bill", ax=axes[0])
axes[0].set_title("stripplot: points jittered sideways (they can overlap)")

sns.swarmplot(data=tips, x="day", y="total_bill", ax=axes[1])
axes[1].set_title("swarmplot: points spread out so none overlap")

plt.show()
```

`jitter=` controls how far strip-plot points spread sideways. Swarm plots show the shape of each group nicely, but they get slow and crowded past a few hundred points per group.

### 4.2 Distributions per group: box, violin and boxen

#### How to read a box plot

```
          o        ← outliers: points beyond the whiskers
          o
        ──┬──      ← upper whisker: largest value within Q3 + 1.5 × IQR
          │
      ┌───┴───┐    ← Q3 (75th percentile): 75% of values are below this
      │       │
      ├───────┤    ← median (50th percentile): the middle value
      │       │
      └───┬───┘    ← Q1 (25th percentile): 25% of values are below this
          │
        ──┴──      ← lower whisker: smallest value within Q1 − 1.5 × IQR
```

**IQR** (interquartile range) = Q3 − Q1 = the height of the box, where the middle 50% of the data lives. A long whisker or many outliers on one side means the data is **skewed** that way.

```python
fig, axes = plt.subplots(1, 3, figsize=(17, 4.3), sharey=True)

sns.boxplot(data=tips, x="day", y="total_bill", ax=axes[0])
axes[0].set_title("boxplot: median, quartiles, outliers")

sns.violinplot(data=tips, x="day", y="total_bill", ax=axes[1])
axes[1].set_title("violinplot: full shape (KDE) + a mini box")

sns.boxenplot(data=tips, x="day", y="total_bill", ax=axes[2])
axes[2].set_title("boxenplot: more quantiles, for big data")

plt.show()
```

- **Violin plot** = a box plot's job done with a smooth density curve (a KDE — see Lesson 5) mirrored on both sides: wide where many values are, thin where few. It shows things a box hides, like two peaks. The curve can extend past the real minimum/maximum; `cut=0` stops it at the data.
- **Boxen plot** (letter-value plot) draws more and more quantile boxes toward the tails — useful with thousands of points, where a box plot marks far too many "outliers".

#### A second grouping with `hue`

```python
fig, axes = plt.subplots(1, 2, figsize=(15, 4.3), sharey=True)

sns.boxplot(data=tips, x="day", y="total_bill", hue="smoker", ax=axes[0])
axes[0].set_title("hue → side-by-side boxes")

sns.violinplot(data=tips, x="day", y="total_bill", hue="smoker",
               split=True, inner="quart", ax=axes[1])
axes[1].set_title("split=True: one half of each violin per hue level")
sns.move_legend(axes[1], "upper left", bbox_to_anchor=(1, 1))   # keep the legend off the violins

plt.show()
```

#### Summary *and* raw data together

A box plot hides how many observations are behind it. Drawing a strip plot on top shows both — the summary and every point:

```python
fig, ax = plt.subplots(figsize=(8, 4.5))
sns.boxplot(data=tips, x="day", y="total_bill", color="lightsteelblue",
            showfliers=False, ax=ax)              # hide outlier markers: the strip shows every point anyway
sns.stripplot(data=tips, x="day", y="total_bill", color="black",
              size=3, alpha=0.5, jitter=0.2, ax=ax)
ax.set(title="Box plot with every bill drawn on top", xlabel="", ylabel="Total bill ($)")
plt.show()
```

### 4.3 Estimates per group: bar, point and count

#### `barplot` — the **average** of each group (not the count!)

The bar height is the **mean** of y for each category (change it with `estimator=`, e.g. `"median"` or `"sum"`). The thin black line is the **95% confidence interval** (change it with `errorbar=`).

In `titanic`, `survived` is 1 (survived) or 0 (died), so **the mean of `survived` is the survival rate**:

```python
titanic[["survived", "pclass", "class", "sex", "age", "fare", "who", "embark_town", "alive"]].head()
```

Output:

```text
   survived  pclass  class     sex   age     fare    who  embark_town alive
0         0       3  Third    male  22.0   7.2500    man  Southampton    no
1         1       1  First  female  38.0  71.2833  woman    Cherbourg   yes
2         1       3  Third  female  26.0   7.9250  woman  Southampton   yes
3         1       1  First  female  35.0  53.1000  woman  Southampton   yes
4         0       3  Third    male  35.0   8.0500    man  Southampton    no
```

```python
fig, ax = plt.subplots(figsize=(8, 4.5))
sns.barplot(data=titanic, x="class", y="survived", hue="sex", ax=ax)
ax.set(title="Titanic: survival rate by class and sex", xlabel="Passenger class", ylabel="Survived")
ax.yaxis.set_major_formatter(mpl.ticker.PercentFormatter(1.0))   # show 0.6 as 60%
plt.show()
```

#### `pointplot` — the same estimate as points joined by lines

The joining lines make it easy to compare **how the pattern changes** between groups: if the lines aren't parallel, the effect of class is different for men and women (an *interaction*).

```python
fig, ax = plt.subplots(figsize=(8, 4.5))
sns.pointplot(data=titanic, x="class", y="survived", hue="sex",
              markers=["o", "s"], linestyles=["-", "--"], ax=ax)
ax.set(title="Titanic: survival rate by class and sex", xlabel="Passenger class", ylabel="Survived")
ax.yaxis.set_major_formatter(mpl.ticker.PercentFormatter(1.0))
plt.show()
```

#### `countplot` — how many rows fall in each category

`countplot` is a histogram for categories: there's no y column, the bar height is the **number of rows**. `ax.bar_label()` writes the value on each bar:

```python
fig, ax = plt.subplots(figsize=(8, 4.5))
sns.countplot(data=titanic, x="class", hue="alive", ax=ax)
for bars in ax.containers:          # one container of bars per hue level
    ax.bar_label(bars)
ax.set(title="Titanic passengers by class and outcome", xlabel="Passenger class", ylabel="Passengers")
plt.show()
```

### 4.4 Order, orientation and panels

- `order=[...]` sets the category order — sort by value, or keep a natural order such as weekdays.
- Put the **categorical variable on the y-axis** to get horizontal bars or boxes — much easier to read with long labels.
- `catplot(..., col=...)` makes small multiples, exactly like `relplot`.

```python
# Horizontal and sorted: highest average fare at the top
order = titanic.groupby("embark_town")["fare"].mean().sort_values(ascending=False).index

fig, ax = plt.subplots(figsize=(8, 3.2))
sns.barplot(data=titanic, y="embark_town", x="fare", order=order, color="steelblue", ax=ax)
ax.set(title="Average fare by port of embarkation", xlabel="Average fare (£)", ylabel="")
plt.show()
```

```python
# Figure-level catplot: one panel per value of 'who' (man / woman / child)
g = sns.catplot(data=titanic, kind="bar", x="class", y="survived", col="who",
                height=3.6, aspect=0.95)
g.set_axis_labels("", "Survived")
g.set_titles("{col_name}")
for ax in g.axes.flat:
    ax.yaxis.set_major_formatter(mpl.ticker.PercentFormatter(1.0))
plt.show()
```

> **seaborn 0.13 note:** giving `palette=` without `hue=` now shows a warning. To give each bar its own color, map the same column to `hue` and hide the redundant legend:
> `sns.barplot(data=tips, x="day", y="tip", hue="day", palette="Set2", legend=False)`.
> For one color for every bar, use `color="steelblue"`.

> [!TIP]
> **Recap — Lesson 4**
> - `catplot(kind=...)` → strip / swarm (every point), box / violin / boxen (spread), bar / point / count (one estimate per group).
> - Box plot: median line, Q1–Q3 box, whiskers to 1.5 × IQR, outliers as dots.
> - `barplot` shows the **mean** + 95% CI; `countplot` shows the **number of rows**.
> - `hue` adds a second grouping (`dodge`, `split=True` for violins); `order=` sorts; put categories on y for horizontal plots.

## 5. Distribution Plots in Seaborn

A distribution plot answers: **which values does a variable take, and how often?** Look for:
- **center** — where most values are
- **spread** — narrow or wide
- **shape** — symmetric or skewed; one peak or several
- **outliers** — values far from the rest

| Axes-level function | `displot(kind=)` | Draws |
|---|---|---|
| `histplot` | `"hist"` (default) | bars counting how many values fall in each bin |
| `kdeplot` | `"kde"` | a smooth density curve (Kernel Density Estimate) |
| `ecdfplot` | `"ecdf"` | the share of values ≤ x, rising from 0 to 1 |
| `rugplot` | `rug=True` | a small tick for every observation |

> `sns.distplot()` from older tutorials is **deprecated** (since 0.11) — use `histplot` or `displot`.

The examples use `penguins`: measurements of 344 penguins of three species.

```python
penguins.head()
```

Output:

```text
  species     island  bill_length_mm  bill_depth_mm  flipper_length_mm  \
0  Adelie  Torgersen            39.1           18.7              181.0   
1  Adelie  Torgersen            39.5           17.4              186.0   
2  Adelie  Torgersen            40.3           18.0              195.0   
3  Adelie  Torgersen             NaN            NaN                NaN   
4  Adelie  Torgersen            36.7           19.3              193.0   

   body_mass_g     sex  
0       3750.0    Male  
1       3800.0  Female  
2       3250.0  Female  
3          NaN     NaN  
4       3450.0  Female  
```

### 5.1 Histograms

```python
fig, axes = plt.subplots(1, 3, figsize=(16, 4))

sns.histplot(data=penguins, x="flipper_length_mm", ax=axes[0])
axes[0].set_title("Default bins")

sns.histplot(data=penguins, x="flipper_length_mm", bins=50, ax=axes[1])
axes[1].set_title("bins=50: too many, mostly noise")

sns.histplot(data=penguins, x="flipper_length_mm", binwidth=4, kde=True, ax=axes[2])
axes[2].set_title("binwidth=4 + kde=True")

plt.show()
```

The bin size changes the story, so try a few. Two peaks (above, right) usually mean the data mixes **different groups** — here, penguin species.

| Argument | Options | Effect |
|---|---|---|
| `bins` / `binwidth` | number of bins / width of each bin | resolution |
| `stat` | `"count"` (default), `"frequency"`, `"probability"` (`"proportion"`), `"percent"`, `"density"` | what the bar height means |
| `kde` | `True` | add a smooth density curve on top |
| `multiple` | `"layer"` (default), `"stack"`, `"dodge"`, `"fill"` | how `hue` groups are combined |
| `element` | `"bars"` (default), `"step"`, `"poly"` | how the bars are drawn |
| `discrete` | `True` | one bar per whole-number value (e.g. party size) |

#### Comparing groups with `hue` and `multiple`

```python
fig, axes = plt.subplots(1, 3, figsize=(17, 4.2))
for ax, multiple in zip(axes, ["layer", "stack", "fill"]):
    sns.histplot(data=penguins, x="flipper_length_mm", hue="species",
                 multiple=multiple, legend=(multiple == "layer"), ax=ax)
    ax.set_title(f'multiple="{multiple}"')
axes[2].set_ylabel("Proportion")      # with "fill", the y-axis is a share, not a count
plt.show()
```

- **layer** — overlapping, semi-transparent: compare the groups' shapes
- **stack** — piled on top of each other: see the total and each group's share of it
- **fill** — every bin stretched to 100%: see the *proportion* of each group at each value

### 5.2 KDE — smooth density curves

A **Kernel Density Estimate** places a small smooth bump on every observation and adds them up, giving a smooth curve instead of bars. The area under the curve is 1. `bw_adjust` is the smoothing knob — the KDE version of bin size:

```python
fig, axes = plt.subplots(1, 3, figsize=(16, 4))

sns.kdeplot(data=penguins, x="flipper_length_mm", bw_adjust=0.25, ax=axes[0])
axes[0].set_title("bw_adjust=0.25: too wiggly")

sns.kdeplot(data=penguins, x="flipper_length_mm", ax=axes[1])
axes[1].set_title("default smoothing (bw_adjust=1)")

sns.kdeplot(data=penguins, x="flipper_length_mm", hue="species", fill=True,
            common_norm=False, alpha=0.4, ax=axes[2])
axes[2].set_title("hue + fill, each curve scaled on its own")

plt.show()
```

`common_norm=False` scales each group's curve to an area of 1 on its own — use it to compare **shapes** when groups have different sizes. With the default `common_norm=True`, all curves together have area 1, so bigger groups get taller curves.

> A KDE assumes a smooth, continuous variable, and it can draw density where there is **no data** (e.g. below the minimum). Check it against a histogram, or add `cut=0`.

### 5.3 ECDF — the cumulative view

An ECDF plots, for every value x, the **share of observations ≤ x**. There are no bins or smoothing to choose, and percentiles can be read straight off the y-axis: where a curve crosses 0.5 is that group's **median**.

```python
fig, ax = plt.subplots(figsize=(8, 4.2))
sns.ecdfplot(data=penguins, x="flipper_length_mm", hue="species", ax=ax)
ax.axhline(0.5, color="gray", linewidth=1)
ax.text(171, 0.52, "median (0.5)", color="gray")
ax.set(title="ECDF: Gentoo penguins have clearly longer flippers",
       xlabel="Flipper length (mm)", ylabel="Share of penguins ≤ x")
plt.show()
```

### 5.4 Figure-level `displot`, rug plots and two variables at once

```python
# displot: kind= picks hist / kde / ecdf, col= makes panels, rug=True adds a tick per penguin
sns.displot(data=penguins, x="flipper_length_mm", col="species", kind="kde",
            fill=True, rug=True, height=3.2, aspect=1.2)
plt.show()
```

The Chinstrap curve is lower because, by default, the densities are scaled across **all** panels together (`common_norm=True`) and there are fewer Chinstrap penguins. Add `common_norm=False` to scale each panel on its own.

Give **both** `x` and `y` to see a **bivariate** (2-D) distribution — a 2-D histogram (color = how many penguins in each cell) or KDE contour lines (read them like the height lines on a hiking map):

```python
fig, axes = plt.subplots(1, 2, figsize=(14, 4.8))

sns.histplot(data=penguins, x="bill_length_mm", y="bill_depth_mm", bins=25,
             cbar=True, ax=axes[0])
axes[0].set_title("2-D histogram")

sns.kdeplot(data=penguins, x="bill_length_mm", y="bill_depth_mm", hue="species",
            levels=5, ax=axes[1])
axes[1].set_title("2-D KDE contours, one color per species")

plt.show()
```

### 5.5 `jointplot` and `pairplot` — many views at once

**`jointplot`** = a two-variable plot in the middle + each variable's distribution along the edges:

```python
sns.jointplot(data=penguins, x="bill_length_mm", y="bill_depth_mm", hue="species", height=5.5)
plt.show()
```

`kind=` options: `"scatter"` (default), `"kde"`, `"hist"`, `"hex"` (hexagonal bins — good for big data) and `"reg"` (adds a regression line).

**`pairplot`** draws a scatter plot for **every pair** of numeric columns, with each column's distribution on the diagonal — the fastest first look at a new dataset. `corner=True` skips the mirror-image upper half:

```python
sns.pairplot(data=penguins, hue="species", corner=True, height=2.3)
plt.show()
```

> [!TIP]
> **Recap — Lesson 5**
> - `displot(kind="hist" | "kde" | "ecdf")` → `histplot`, `kdeplot`, `ecdfplot`, plus `rugplot` / `rug=True`.
> - Histograms depend on `bins` / `binwidth`; KDEs on `bw_adjust` — always try a few values.
> - With `hue`: `multiple="layer" | "stack" | "fill"` for histograms, `common_norm=False` to compare shapes.
> - ECDF: no tuning, read medians and percentiles directly.
> - Two variables: `histplot` / `kdeplot` with x and y, `jointplot`; every pair: `pairplot`.

## 6. Relational Plots in Seaborn — Heatmaps

A **heatmap** shows a table of numbers as a grid of colored cells: rows × columns → color. It shows how a value relates to **two** other variables at once, which makes it ideal for:
- a value across two categories or time scales — passengers by **month × year**
- **correlation matrices** — how strongly every numeric column relates to every other

(The Seaborn docs list `heatmap` under "matrix plots". It is axes-level, so it accepts `ax=`.)

### 6.1 Step 1 — get the data into matrix (wide) form

Unlike the other functions, `heatmap` does **not** take `x=` / `y=` column names. It wants a **rectangular table** where:
- the **index** becomes the rows of the heatmap
- the **columns** become its columns
- the **values** become the cell colors

Build that table with `pivot` (one value per cell), `pivot_table` (combine several values per cell), `pd.crosstab` (counts) or `df.corr()` (correlations).

```python
flights_matrix = flights.pivot(index="month", columns="year", values="passengers")
flights_matrix
```

Output:

```text
year   1949  1950  1951  1952  1953  1954  1955  1956  1957  1958  1959  1960
month                                                                        
Jan     112   115   145   171   196   204   242   284   315   340   360   417
Feb     118   126   150   180   196   188   233   277   301   318   342   391
Mar     132   141   178   193   236   235   267   317   356   362   406   419
Apr     129   135   163   181   235   227   269   313   348   348   396   461
May     121   125   172   183   229   234   270   318   355   363   420   472
Jun     135   149   178   218   243   264   315   374   422   435   472   535
Jul     148   170   199   230   264   302   364   413   465   491   548   622
Aug     148   170   199   242   272   293   347   405   467   505   559   606
Sep     136   158   184   209   237   259   312   355   404   404   463   508
Oct     119   133   162   191   211   229   274   306   347   359   407   461
Nov     104   114   146   172   180   203   237   271   305   310   362   390
Dec     118   140   166   194   201   229   278   306   336   337   405   432
```

### 6.2 Step 2 — draw it

```python
fig, ax = plt.subplots(figsize=(10, 6))
sns.heatmap(flights_matrix, ax=ax)
plt.show()
```

Two patterns jump out: traffic grows every year (colors get lighter to the right), and every year peaks in summer (the July and August rows). Now make it easier to read:

```python
fig, ax = plt.subplots(figsize=(12, 7))
sns.heatmap(
    flights_matrix,
    annot=True, fmt="d",                    # write each value in its cell; "d" = whole number
    cmap="YlGnBu",                          # sequential: light = few passengers, dark = many
    linewidths=0.5, linecolor="white",      # thin gaps between cells
    cbar_kws={"label": "Passengers (thousands)"},
    ax=ax,
)
ax.set(title="Monthly airline passengers, 1949–1960", xlabel="Year", ylabel="")
plt.show()
```

| Argument | Example | What it does |
|---|---|---|
| `annot` | `True` | write the value in each cell |
| `fmt` | `"d"`, `".1f"`, `".0%"` | number format for those values |
| `cmap` | `"rocket"`, `"mako"`, `"YlGnBu"`, `"vlag"` | colormap — sequential for amounts, diverging around a midpoint |
| `vmin`, `vmax` | `vmin=0, vmax=1` | fix the two ends of the color scale |
| `center` | `center=0` | the value that gets the middle color (use with a diverging colormap) |
| `linewidths`, `linecolor` | `0.5`, `"white"` | gaps between the cells |
| `cbar`, `cbar_kws` | `False`, `{"label": "…"}` | hide / label the color bar |
| `square` | `True` | square cells |
| `mask` | a True/False array | hide the cells where the mask is `True` |
| `robust` | `True` | set the color range from percentiles, so a few extreme values don't wash out the rest |

### 6.3 Combining values with `pivot_table`

When several rows fall into the same cell, `pivot_table` combines them (`aggfunc="mean"` by default). Titanic survival rate by sex and class:

```python
survival = titanic.pivot_table(index="sex", columns="class", values="survived",
                               aggfunc="mean", observed=True)

fig, ax = plt.subplots(figsize=(7, 3))
sns.heatmap(survival, annot=True, fmt=".0%", cmap="rocket_r", vmin=0, vmax=1, linewidths=1,
            cbar_kws={"label": "Survival rate", "format": mpl.ticker.PercentFormatter(1.0)},
            ax=ax)
ax.set(title="Titanic survival rate", xlabel="", ylabel="")
plt.show()
```

### 6.4 Correlation heatmaps

`df.corr()` gives the correlation between every pair of numeric columns, already in matrix form. A correlation runs from **−1** (perfect negative) through **0** (no linear relationship) to **+1** (perfect positive), so use a **diverging** colormap with `center=0` and fix the scale with `vmin=-1, vmax=1`:

```python
corr = penguins.corr(numeric_only=True)    # numeric_only=True skips text columns like 'species'
corr.round(2)
```

Output:

```text
                   bill_length_mm  bill_depth_mm  flipper_length_mm  \
bill_length_mm               1.00          -0.24               0.66   
bill_depth_mm               -0.24           1.00              -0.58   
flipper_length_mm            0.66          -0.58               1.00   
body_mass_g                  0.60          -0.47               0.87   

                   body_mass_g  
bill_length_mm            0.60  
bill_depth_mm            -0.47  
flipper_length_mm         0.87  
body_mass_g               1.00  
```

```python
fig, ax = plt.subplots(figsize=(7.5, 6))
sns.heatmap(corr, annot=True, fmt=".2f", cmap="vlag", center=0, vmin=-1, vmax=1,
            square=True, linewidths=0.5, cbar_kws={"label": "Correlation"}, ax=ax)
ax.set_title("Correlation between penguin measurements")
plt.show()
```

A correlation matrix is symmetric (corr(A, B) = corr(B, A)) and its diagonal is always 1, so half of it repeats the other half. A **mask** hides the upper triangle:

```python
mask = np.triu(np.ones_like(corr, dtype=bool))   # True on and above the diagonal → hidden

with sns.axes_style("white"):                      # plain background, so hidden cells are blank
    fig, ax = plt.subplots(figsize=(7.5, 6))
    sns.heatmap(corr, mask=mask, annot=True, fmt=".2f", cmap="vlag", center=0, vmin=-1, vmax=1,
                square=True, linewidths=0.5, cbar_kws={"label": "Correlation", "shrink": 0.8}, ax=ax)
    ax.set_title("Correlation, lower triangle only")
    plt.show()
```

**Reading it:** flipper length, body mass and bill length rise together (strong positive correlations). Bill depth is *negatively* correlated with the others across all penguins — but **within each species** the relationship is positive. The negative overall number comes from mixing species (Gentoo penguins are big but have shallow bills). This is called **Simpson's paradox**, and it's why a correlation heatmap should be checked against scatter plots split by group (like the `pairplot` in Lesson 5):

```python
print("Correlation of bill length with bill depth")
print(f"{'All penguins':13}", round(penguins["bill_length_mm"].corr(penguins["bill_depth_mm"]), 2))
for species, group in penguins.groupby("species"):
    print(f"{species:13}", round(group["bill_length_mm"].corr(group["bill_depth_mm"]), 2))
```

Output:

```text
Correlation of bill length with bill depth
All penguins  -0.24
Adelie        0.39
Chinstrap     0.65
Gentoo        0.64
```

> **Related:** `sns.clustermap()` draws a heatmap and **reorders** its rows and columns so that similar ones sit next to each other (hierarchical clustering) — handy for spotting groups in bigger matrices.

> [!TIP]
> **Recap — Lesson 6**
> - A heatmap needs a **matrix**: reshape with `pivot`, `pivot_table`, `pd.crosstab` or `df.corr()`.
> - `annot=True` + `fmt` writes the values; `cmap` sets the colors; `linewidths` adds gaps; `cbar_kws` labels the color bar.
> - Sequential colormap for amounts; diverging colormap + `center=0` (+ `vmin=-1, vmax=1`) for correlations.
> - Use `mask` to hide the repeated half of a correlation matrix, and don't trust a correlation without looking at the scatter plot.

## 7. Best Practices for Data Visualization

A chart is good when the reader gets the **right message, quickly**. These guidelines hold for any tool; each one comes with the Seaborn way to do it.

### 7.1 Know the purpose and the audience

- **Exploratory** charts are for *you*: quick, many, rough — `pairplot`, default settings, no polishing.
- **Explanatory** charts are for *others*: one clear message per chart, readable labels, and a title that states the finding.

### 7.2 Choose the chart by the question

| Question | Good chart | Seaborn |
|---|---|---|
| How are one variable's values spread? | histogram, KDE, box, violin, ECDF | `histplot`, `kdeplot`, `ecdfplot`, `boxplot`, `displot` |
| How do groups compare on a number? | bar / point with error bars, box, violin | `barplot`, `pointplot`, `boxplot`, `catplot` |
| How many are in each group? | bar chart of counts | `countplot` |
| How do two numbers relate? | scatter (+ trend line) | `scatterplot`, `regplot`, `lmplot` |
| How does something change over time? | line | `lineplot` |
| A value across two dimensions, or correlations | heatmap | `heatmap` |
| First look at many variables | pair plot | `pairplot` |
| Parts of a whole | stacked or 100% bar — avoid pie charts when the slices are close in size | `histplot(multiple="fill")`, pandas `df.plot.bar(stacked=True)` |

### 7.3 Be honest with scales

- **Bar charts start at zero.** A bar's *length* is the value, so a cut-off axis exaggerates differences. (Points and lines may zoom in, because they encode position, not length.)
- **Avoid two different y-axes on one plot.** How the two scales line up is arbitrary, so any pattern between the lines is invented. Use two plots or small multiples.
- **Keep shared axes across panels** (the default in `relplot`, `catplot`, `displot`) so panels can be compared at a glance.
- **Show uncertainty and spread**, not just averages: keep the error bars, or show the distribution (box, violin, strip).

#### Example: the same data, misleading vs. honest

```python
tip_rate = tips.assign(tip_pct=tips["tip"] / tips["total_bill"] * 100)   # tip as % of the bill

fig, (bad, good) = plt.subplots(1, 2, figsize=(14, 4.5))

# BEFORE: rainbow colors on unordered bars, axis cut off at 14.5, raw column names, no message
sns.barplot(data=tip_rate, x="day", y="tip_pct", hue="day", palette="hsv",
            legend=False, errorbar=None, ax=bad)
bad.set_ylim(14.5, 17.5)
bad.set_title("BEFORE")

# AFTER: one color, axis from zero, readable labels, error bars, a title with the takeaway
sns.barplot(data=tip_rate, x="day", y="tip_pct", color="steelblue", ax=good)
good.set(xlabel="", ylabel="Average tip (% of bill)",
         title="AFTER: tip rates are about the same every day (15–17%)")

plt.show()
```

On the left, Friday looks like it tips about three times more than Saturday. The real difference is under 2 percentage points (17.0% vs. 15.3%), and on the right the overlapping error bars show it may well be noise.

### 7.4 Use color on purpose

- **Match the palette type to the data** (Lesson 1): qualitative for unordered groups, sequential (one hue, light → dark) for ordered amounts, diverging (two hues with a neutral middle) when there's a meaningful center such as 0.
- **Avoid rainbow colormaps** (`"jet"`, `"hsv"`). They create fake boundaries and can't be read in grayscale or by many colorblind viewers. Use `"viridis"`, `"rocket"`, `"mako"` or `"crest"`. (Seaborn refuses `palette="jet"` outright — it raises an error.)
- **Don't color what's already shown.** Bars for unordered categories need *one* color — a gradient suggests an order that isn't there.
- **Keep the number of colors small** — about 7 at most. With more groups, merge the small ones into "Other" or use panels (`col=`).
- **Plan for color blindness** (about 1 in 12 men): use `palette="colorblind"`, and add a second cue — `style=`, markers or direct labels — so the meaning never depends on color alone.
- **Same category, same color, in every chart** of a report: pass a fixed dict, e.g. `palette={"Lunch": "C1", "Dinner": "C0"}`, and a fixed `hue_order`.
- **Use emphasis:** gray out the context and color only what the reader should look at.

```python
fig, (bad, good) = plt.subplots(1, 2, figsize=(15, 4.8), sharey=True)

# BEFORE: 12 colored lines and a 12-entry legend — nothing stands out
sns.lineplot(data=flights, x="year", y="passengers", hue="month", palette="tab20", ax=bad)
sns.move_legend(bad, "upper left", ncol=2, fontsize=8)
bad.set_title("BEFORE: 12 colors, no story")

# AFTER: every month in light gray as context, one highlighted line carries the message
sns.lineplot(data=flights, x="year", y="passengers", hue="month",
             palette=["lightgray"] * 12, legend=False, linewidth=1, ax=good)
july = flights[flights["month"] == "Jul"]
sns.lineplot(data=july, x="year", y="passengers", color="crimson", linewidth=2.5, ax=good)

first, last = july["passengers"].iloc[0], july["passengers"].iloc[-1]
good.annotate(f"July (peak season):\n{first}k → {last}k passengers",
              xy=(1960, last), xytext=(1953.2, 560), color="crimson",
              arrowprops=dict(arrowstyle="->", color="crimson"))
good.set(title="AFTER: gray context, one highlighted line",
         xlabel="Year", ylabel="Passengers (thousands)")
plt.show()
```

### 7.5 Cut the clutter and handle overplotting

- Remove what carries no information: heavy borders, unneeded gridlines, 3-D effects, busy backgrounds. `style="whitegrid"` or `style="ticks"` + `sns.despine()` are clean defaults.
- Don't write a number on every point. Label only what matters: `ax.bar_label` for a few bars, `ax.annotate` for one key point.
- **Too many points?** Dots pile up into a solid blob and hide where most of the data is. Fixes: transparency (`alpha`), smaller markers, or a density view (`histplot` with x and y, `jointplot(kind="hex")`, `kdeplot`).

```python
fig, axes = plt.subplots(1, 3, figsize=(17, 4.4), sharex=True, sharey=True)

sns.scatterplot(data=diamonds, x="carat", y="price", ax=axes[0])
axes[0].set_title("BEFORE: 54,000 solid dots = one blob")

sns.scatterplot(data=diamonds, x="carat", y="price", s=4, alpha=0.1, linewidth=0, ax=axes[1])
axes[1].set_title("AFTER: small, transparent dots")

sns.histplot(data=diamonds, x="carat", y="price", bins=60, cbar=True,
             pmax=0.8,                 # color scale tops out early, so the busy cells don't hide the rest
             ax=axes[2])
axes[2].set_title("AFTER: 2-D histogram (color = how many)")

for ax in axes:
    ax.set(xlabel="Carat", ylabel="Price ($)")
plt.show()
```

### 7.6 Order, labels and the final polish

- **Sort bars by value** when the categories have no natural order (`order=` — Lesson 4); keep the natural order when they do (weekdays, months, sizes).
- **Readable labels:** replace raw column names like `total_bill` with "Total bill ($)", include **units**, and write titles that state the finding ("Bigger bills get bigger tips") rather than just naming the variables ("tip vs total_bill").
- **Horizontal bars** when category names are long.
- **Size text for where it will be shown:** `sns.set_context("talk")` for slides, `"paper"` for documents; export with `dpi=300` (or `.svg` / `.pdf`).

Putting it all together — a presentation-ready chart:

```python
slope = np.polyfit(tips["total_bill"], tips["tip"], 1)[0]   # extra tip per extra dollar of bill

with sns.axes_style("ticks"), sns.plotting_context("talk"):  # clean style, slide-sized text
    fig, ax = plt.subplots(figsize=(10, 6))
    sns.scatterplot(data=tips, x="total_bill", y="tip", hue="time", style="time",
                    palette={"Lunch": "C1", "Dinner": "C0"}, s=70, alpha=0.8, ax=ax)
    sns.regplot(data=tips, x="total_bill", y="tip", scatter=False, color="gray",
                line_kws={"linewidth": 1.5}, ax=ax)
    ax.set(title=f"Tips grow with the bill: about {slope * 100:.0f}¢ per extra dollar",
           xlabel="Total bill ($)", ylabel="Tip ($)")
    sns.move_legend(ax, "upper left", title=None, frameon=False)
    sns.despine()
    plt.show()
```

### 7.7 Checklist before you share a chart

- [ ] The chart type fits the question
- [ ] There is **one clear message**, and the title says it
- [ ] Axis labels are words **with units** — no raw column names
- [ ] Bar charts start at **zero**; no double y-axes
- [ ] Every color has a job (group / amount / above-below a midpoint) and the palette type matches it
- [ ] About 7 colors at most, colorblind-safe, and meaning doesn't rely on color alone
- [ ] Uncertainty or spread is shown where it matters (error bars, box, violin)
- [ ] Overplotting handled (transparency, 2-D histogram, hexbin)
- [ ] Categories are in a meaningful order
- [ ] A legend is present when there are several groups, and nothing is cut off
- [ ] Text is large enough for where it will be shown; exported at high resolution

> [!TIP]
> **Recap — Lesson 7**
> Pick the chart from the question, keep scales honest, use color for a reason, remove clutter, label in plain words with units, and put the takeaway in the title.

## Cheat Sheet

#### Which function?

| I want to show… | Figure-level | Axes-level |
|---|---|---|
| the relationship between two numbers | `relplot(kind="scatter")` | `scatterplot` |
| a trend over time / an ordered x | `relplot(kind="line")` | `lineplot` |
| a fitted trend line | `lmplot` | `regplot` |
| the distribution of one variable | `displot(kind="hist" / "kde" / "ecdf")` | `histplot`, `kdeplot`, `ecdfplot`, `rugplot` |
| the distribution of two variables | `displot(x=..., y=...)`, `jointplot` | `histplot(x=..., y=...)`, `kdeplot(x=..., y=...)` |
| a number across categories — every point | `catplot(kind="strip" / "swarm")` | `stripplot`, `swarmplot` |
| a number across categories — the spread | `catplot(kind="box" / "violin" / "boxen")` | `boxplot`, `violinplot`, `boxenplot` |
| an average per category | `catplot(kind="bar" / "point")` | `barplot`, `pointplot` |
| counts per category | `catplot(kind="count")` | `countplot` |
| a matrix of values / correlations | `clustermap` | `heatmap` |
| every pair of numeric columns | `pairplot` | — |

#### Arguments you'll use all the time

| Argument | Meaning |
|---|---|
| `data`, `x`, `y` | the DataFrame and the column names |
| `hue` | color by a column |
| `size`, `style` | marker size / marker shape or line dashes (relational plots) |
| `col`, `row`, `col_wrap` | panels — figure-level functions only |
| `height`, `aspect` | panel size — figure-level functions only |
| `ax` | which subplot to draw on — axes-level functions only |
| `kind` | the plot type inside `relplot`, `displot`, `catplot`, `jointplot` |
| `order`, `hue_order` | the order of the categories |
| `palette`, `color` | colors for the `hue` groups / one single color |
| `estimator`, `errorbar` | how values are aggregated and what uncertainty is shown (line, bar, point plots) |
| `legend` | `False` to hide it |

#### Old → new names (when following older tutorials)

| Old | Use now |
|---|---|
| `sns.distplot(x)` | `sns.histplot(x, kde=True)` or `sns.displot(...)` |
| `sns.factorplot(...)` | `sns.catplot(...)` |
| `sns.tsplot(...)` | `sns.lineplot(...)` |
| `ci=95`, `ci="sd"`, `ci=None` in line / bar / point plots | `errorbar=("ci", 95)`, `errorbar="sd"`, `errorbar=None` |
| `size=` in figure-level functions | `height=` |
| `g.fig` | `g.figure` |
| `palette=` without `hue=` | `hue=<same column>, legend=False`, or `color=` for one color |
| `sns.set()` | `sns.set_theme()` (the old name still works) |

#### Going further: the objects interface

Since version 0.12, Seaborn also has a newer, "build it piece by piece" interface, `seaborn.objects`. You start with `so.Plot(...)` and **add** layers. It's optional — everything above still works — but it's where the library is heading:

```python
import seaborn.objects as so

(
    so.Plot(tips, x="total_bill", y="tip", color="time")
    .add(so.Dot(alpha=0.6))                      # layer 1: the points
    .add(so.Line(), so.PolyFit(order=1))         # layer 2: a straight-line fit per group
    .label(x="Total bill ($)", y="Tip ($)", color="Time")
    .layout(size=(8, 4.5))                       # figure size in inches
)
```

#### Official resources
- Tutorial: <https://seaborn.pydata.org/tutorial.html>
- Every function, with examples: <https://seaborn.pydata.org/api.html>
- Example gallery: <https://seaborn.pydata.org/examples/index.html>

---

## Author

[@Preetbbhalala](https://github.com/Preetbbhalala) — notes written while learning Seaborn.
