# CM Hub: Data processing with Pandas
Data processing with Pandas course for the CM Hub at Imperial College

2 × 2 hour classes

## The Graduate School logo
<img src="/readme-images/grad-school-logo.png">

## On completion of this workshop you will be able to:
* use Jupyter notebooks to perform simple Pandas data analysis,
* apply fundamental components of Pandas syntax including data selection and grouping,
* create programs designed to process example data and display simple statistics,
* interpret common errors and use these to help debug a program.

## Prerequisites
* Students are expected to be familiar with [basic Python](https://www.imperial.ac.uk/computational-methods/cm-hub/intro_python/).
* Students are welcome to bring their own fully charged laptops to these sessions although there are computers in situ.
* If using your own laptop, pleasure ensure you have your preferred Python environment installed (we recommend Anaconda), including the modules `pandas`, `xlrd`, `statsmodels`, `matplotlib`. The instructor will use Jupyter Notebooks for demonstrations.

## YouTube videos for distance learning
Timestamps are in the descriptions and correspond to the numbering in this document.
* Part 1: https://www.youtube.com/watch?v=ozxAW2mm7A0
* Part 2: https://www.youtube.com/watch?v=MZuni39oxr0
* Playlist: https://www.youtube.com/playlist?list=PLcRp2oAQBugtlo4ZjnuzhOCS21SXba4Zq

## Before we begin
* Pandas is a Python package. You need Python working on your system.
* On your own machine:
    * `pip install pandas`
    * `pip install xlrd` (required for pd.read_excel() to work)
    * `pip install statsmodels`
* On the college computers:
    *  `conda install pandas`
    *  `conda install xlrd`
    *  `conda install statsmodels`
    *  Restart the kernel (click Kernel > Restart)
    
## Part 1

### 1. Why you might want to use Pandas
Pros of Pandas:
* Is part of Python and Python is great
* Good for large data sets
* Powerful tools for dealing with broken data (etc.)

Cons of Pandas:
* You need to know Python
* No graphical interface
* You need a few extra packages
* Eats shoots and leaves

In short: The setup cost is often worthwhile to allow you powerful data manipulation features, which you can then connect to some useful Python code.

### 2. Let's get started
* Join in. Open a Jupyter notebook by typing `jupyter notebook` in the terminal.
* Jupyter notebook specific: We're going to want to display our graphs inside the notebook, rather than externally. Do to that, type `%matplotlib inline`.
* We're going to be using Python 3. If you've got Python 2, that's OK, just start by typing
```python
from __future__ import print_function
```
* `print("hello world")`

Test it works!

* `import pandas as pd`
* `import matplotlib.pyplot as plt`

OK let's go

* `data = pd.read_csv("A1_mosquito_data.csv")`
* `data` (fine within a Jupyter notebook)
* `print(data)` (otherwise)
* `data.head()`
* `data.tail()`
* `data.sample()`
* `data.describe()`

Let's pick out things:

* `data['year']`
* `data[['year','mosquitos']]`
* `data[0:2]`

Your turn:

* Print the last 3 rows
* Print the first two rows, but only the year and rainfall columns
* Try to print a single row

Join in:

* `data[1:2]`
* `data.iloc[1]`
* `data.iloc[1:3]`
* `data['temperature'][0])` (so you can select a single row this way)
* `data['temperature'][data['temperature'] > 75]`
* `data['temperature'][data['year'] > 2005]`
* `data.mean()`
* `data.mean().mean()`
* `data.mean(1))` (not very useful here)
* `data.max()`
* `data.idxmin()`
* `data['temperature'].min()`


Your turn:

* Print the mean number of mosquitos in the years where the rainfall was more than 200mm.
* Now do the same for when the rainfall was less than 200mm.
* Print the standard deviation of the temperatures (you may have to Google!)

### 3. Loops and ifs
These work in the usual Python way:

Join in:
```python
for index, row in data.iterrows():
    temp_in_f = row['temperature']
    temp_in_c = (temp_in_f - 32) * 5 / 9.0
    print(temp_in_c)
```
* We can add in:
```Python
if temp_in_c > 27:
    print("Hotter than 27C!")
```

Your turn:
* Import the data from `A2_mosquito_data.csv` into `data2`, determine the mean temperature, and loop over the temperature values. For each value print out whether it is greater than the mean, less than the mean, or equal to the mean.

### 4. Plots

Join in:

* `mosquitos_vs_year = data[['year','mosquitos']]`
* `mosquitos_vs_year.plot(kind='line')`
* `mosquitos_vs_year.plot(kind='line',x='year')`
* `mosquitos_vs_year.plot(kind='bar',x='year')`

Your turn:

* Using the `data2` dataset, plot the number of mosquitos against the rainfall. What needs fixing?

Join in:
* `mosquitos_vs_rainfall = data[['rainfall','mosquitos']]`
* `mosquitos_vs_rainfall.plot(kind='scatter',x='rainfall',y='mosquitos')`
* `plt.xlabel('Rainfall (mm)')`
* `plt.ylabel('Mosquitos')`
* `plt.title('Mosquitoes like water')`

Your turn:
* Plot the number of mosquitos against the temperature.

Join in:
`plt.savefig('beautiful_graph.pdf')` (directly after plot command in same cell)

### 5. Grouping

Let's group the data by temperature and then plot the mean number of mosquitoes for each temperature (to the nearest degree).

Join in:
* `mosquito_data_only = data[['temperature','mosquitos']]`
* `mosquito_data_only.groupby('temperature').mean().plot(kind='line')`

Your turn:
* Do the same with the larger data file.

### 6. Binning

Binning sorts data into intervals, or 'bins'.

Join in:

```python
bins = [0,200,250,300]
labels = ['dry','normal','wet']
pd.cut(data['rainfall'],bins,labels=labels).value_counts().plot(kind='pie')
```

### 7. Adding columns

We can add columns:

Join in:
* `data['temperature_celsius'] = (data['temperature']-32)*5/9.`
* `data.head()`

### 8. Sorting

We can sort data:

Join in:
* `data.sort_values('temperature')`
* `data.sort_values(['temperature','rainfall'])`
* `data.sort_values('temperature',ascending=False)`
* `data.mean().sort_values()`

## Part 2

### Getting up and running again
* If you want to continue from your previous Jupyter notebook and have managed to re-open it, you will have to click **Cell** > **Run All**.
* To continue in a new Jupyter notebook, place in the top cell `%matplotlib inline` and in the second cell:
```python
import pandas as pd
import matplotlib.pyplot as plt
data = pd.read_csv("A1_mosquito_data.csv")
data2 = pd.read_csv("A2_mosquito_data.csv")
```

### 9. Histograms
* `data.hist()`

And the scatter matrix:
```python
from pandas.plotting import scatter_matrix
scatter_matrix(data[['temperature','rainfall','mosquitos']])
```

Correlation matrix:
* `data.corr()`

### 10. Regression
```Python
import statsmodels.api as sm

x = data["rainfall"]
y = data["mosquitos"]
X = sm.add_constant(x) # add y-intercept

model = sm.OLS(y, X).fit() # Ordinary Least Squares
predictions = model.predict(X)

model.summary()
```

In
```
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const         49.8413      3.223     15.465      0.000      42.409      57.273
rainfall       0.6592      0.015     44.965      0.000       0.625       0.693
```
we see mosquitos = 0.6592*rainfall + 49.8413

To get them out:
* `model.params['const']`
* `model.params['rainfall']`
* `model.pvalues`
etc

### 11. Miscellaneous things

We can ignore missing or NaN values:
* `data.sum(0, skipna=False)`

### 12. Project

* Import from Excel `exam_results.xlsx`

You're the director of undergraduate studies. You have to do a few things.

1. You need to give prizes to the five students taking Physics with the top mean marks over all four modules. Which students get the prizes?
2. The staff member running the tutor group with the highest mean mark gets a beer. Which group's tutor gets the beer?
3. You need to report the mean mark for each course to the faculty. List the four courses by order of mean mark. Plot these on a bar chart so they can understand it.
4. Scores above 70% are a 'first'. Scores between 60 and 69% are an 'upper second', between 50 and 59% a 'lower second', between 40 and 49% a 'third', and 39% and below is a fail. For Quantum Mechanics, plot a pie chart showing the number of students who fall in each of these categories.
5. Students on the Physics programme pass the year if they score more than 40% on three out of four modules. Otherwise they fail. How many students failed? Loop through the failing students, printing out a personalised statement (imagine that you will code it so Python emails it to them) telling them they've failed.
6. Rumour has it the scores for Lab Work have been made up. Create a scatter matrix for the four courses. What does this tell you?
7. Do a linear regression analysis to come up with a linear model for the Waves score based on the Relativity score.

## Feedback form
If you're taking this course through the Graduate School, please fill out [the feedback form](http://bit.ly/computingdatascience1920).

## Licence
Mosquitos example adapted from Software Carpentry. [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

The rest of the work is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International Licence](http://creativecommons.org/licenses/by-nc-sa/4.0/).

