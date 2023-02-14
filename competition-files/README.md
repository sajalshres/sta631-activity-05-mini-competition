Day 1 - Qualitative Explanatory Variables
================

In this repository/directory you should see the following items:

- `README.md` - this document.
- `mini-competition.Rmd` - an empty RMarkdown file for you to keep track
  of your explorations and provide detailed notes.
- `data` - a subfolder containing the dataset.

## The “rules”

You and your team have been “hired” by [Provost Fatma
Mili](https://www.gvsu.edu/provost/) and her office to assess factors
that might contribute to GVSU students’ debt accumulated by the time
they graduate with their bachelor’s degree. You are tasked with
determining the “best” regression model for these data.

By **4:30 pm on Thursday, February 16** you need to create a Google
slide deck that only contains two-slides. At least one member from each
team will provide a 5-minute maximum presentation of these slides. Your
slides should include:

1.  Your final model and a description of how “good” it fits the data
    (you will need to explain how you determined “good-ness”), and
2.  A summary of the process your team took to explore and decide on
    your final model.

Instead of asking questions after each presentation, we will hopefully
have time at the end to have a closing conversation. Also, to help
create slides quickly (so you can focus on the modeling task), we will
use Google Slides instead of creating [slides in
RMarkdown](https://rmarkdown.rstudio.com/lesson-11.html).

Upload your slides to this Google folder by the deadline:
<https://drive.google.com/drive/folders/1kgG_XZd0v8jB2vQueaAwKlt3Ns47HCeI?usp=sharing>

Presentation order will be randomly determined around 4:31 pm and edits
to your slides will not be allowed.

While this is a friendly competition, the main purpose is to share your
ideas/process and learn from your peers and their ideas/process. I
(Bradford) will determine the “best” group based *only* on your
presentation by assessing each of the following categories on a
“missing/attempted/sufficient”-scale (or 0/0.5/1 point-scale; I will be
very [stingy](https://www.merriam-webster.com/dictionary/stingy) with
awarding “1”s except for the last category):

- Clarity of your presentation,
- Soundness of your approach,
- Aesthetically pleasing presentation, and
- Maximum of 5-minutes and two slides (only 0 or 1 possible).

The best group based on these categories will receive a prize/gift. I
hope to have a few more mini-competitions throughout this semester with
more prizes/gifts.

### Data

Note that if you were in my STA 518 section Winter 2021 or before, you
might have already worked with these data. Do your best to avoid looking
at your work from STA 518 as this project served a different purpose.
Also, you have changed/grown since then - focus on what you can now do
instead of what you used to be able to do.

You have an `allendale-students.csv` datafile contained within the
`data/` subfolder of this repo. These data are a *random sample* from
the population of recent GVSU graduates that lived in Allendale,
Michigan and had cars during their time at GVSU. The population is
further restricted to students that had at most 50% of their total
college cost paid by parents.

To help you focus your attention - rather than overwhelm you will
1,000’s of potential explanatory predictors - Assistant Vice President
for Data Analytics Aaron Lowen has provided you with the following
variables:

| Variable      | Description                                                                                            |
|:--------------|:-------------------------------------------------------------------------------------------------------|
| `distance`    | Distance (in miles) from hometown to Allendale                                                         |
| `scholarship` | Financial support (in dollars) from scholarships (average per year)                                    |
| `parents`     | Percent of total cost of bachelor’s degree paid by parents (max of 50%)                                |
| `car`         | Age of car (in years)                                                                                  |
| `housing`     | Type of housing (on campus, off campus). *Treat “off campus” as the reference group*                   |
| `major`       | STEM (Science, Technology, Engineering, Math), business, other. *Treat “other” as the reference group* |
| `debt`        | Student debt (in dollars) accumulated by end of bachelor’s degree. *This is the response variable*     |

### Suggestions to get started

- Make appropriate exploratory graphs and numerical summary tables for
  each variable and pairs of variables. Note that there are some
  qualitative variables in the dataset. Also, do you need to consider
  any potential interaction terms or polynomial terms?
- Explore if you need to do anything to ensure the reference groups
  listed in the variable description table.
- What methods have we explored to assess a model’s “good-ness”? As a
  group, come up with this list and what each tells us. In [Additional
  Methods](#additional-methods) below, I provide some further
  descriptive (i.e., not statistical inference focused) methods to check
  for “good-ness”.
- Fit your candidate models.
- Using the methods from (3), assess which of your candidates is “best”.

## Additional methods

The *ISL* text discusses some methods for checking a model’s “good-ness”
or adjusting for issues when checking the conditions for linear
regression models (i.e., Section 3.3.3). I briefly provide how to
implement or check for these *Potential Problems* (in order as presented
in the text), but remember that the text provides more detail with how
to interpret or know if these adjustments are necessary. Throughout this
section, I provide additional resources that I find useful.

You might have heard of the adjusted $R^2$, *Akaike information
criterion* (AIC), *Bayesian information criterion*, and *Mallow’s* $C_p$
model metrics before. We will explore these in more detail in Chapter 6
(Week 12).

### 1. Non-linearity of the data

If the *errors* display a pattern where linearity might be a concern,
you can transform variables to see if these “fix” this issue. Some
online documents that provide good overviews of transformations are:

- [Transformations: an
  introduction](http://fmwww.bc.edu/repec/bocode/t/transint.html) by
  Nicholas J. Cox, Durham University
- [A guide to Data
  Transformation](https://medium.com/analytics-vidhya/a-guide-to-data-transformation-9e5fa9ae1ca3)
  by Tim M. Schendzielorz
- [Lesson 9: Data
  Transformations](https://online.stat.psu.edu/stat501/lesson/9) by Penn
  State’s Department of Statistics

These would all be done in a data `mutate`-tion step (e.g., using your
`{dplyr}` skills). For example,

``` r
# add natural log transformation to existing dataset
data <- data %>%
  mutate(log_variable =  log(variable))
```

You would then use this transformed variable in your linear regression
model instead of the un-transformed variable.

### 2. Correlation of error terms

This should be an issue for these data and time-series is not an
expected topic for this course. However, for your project you might want
to extend your learning by performing a time-series analysis.

### 3. Non-constant variance of error terms

Transforming a variable (see above) could be one way to address this
issue. A more advanced method would be weighted least squares, but not
necessary for this model - another potential extending your learning
that is covered in Chapter 7.

### 4. Outliers

The `broom::augment` function contains a lot of additional metrics
related to assessing outliers. For example, the text mentions
*studentized residuals* which is the `.std.resid` column in the
resulting tibble from using this function.

You can also visually identify and highlight outliers (e.g.,
[`gghighlight::gghighlight`](https://yutannihilation.github.io/gghighlight/articles/gghighlight.html#gghighlight)
which combines a filtering flavor to point out specific values).

### 5. High leverage points

Again, the `broom::augment` function contains a lot of additional
metrics. The text mentions *leverage statistics* $h_i$ which is the
`.hat` column in the resulting tibble from using this function. You can
then use your `{dplyr}` skills to help you identify any extreme values
(e.g., see the text for how to calculate the suggested cutoff value).

This online document discusses outliers and high leverage points:

- [Lesson 9.1: Distinction Between Outliers and High Leverage
  Observations](https://online.stat.psu.edu/stat462/node/170/) by Drs
  Iain Pardoe, Laura Simon, and Derek Young of Penn State.

### 6. Collinearity

In addition to inspecting a correlation/scatterplot matrix (e.g.,
`GGally::ggpairs`), you can also compute the *variance inflation factor*
(VIF) for each predictor. To compute these values, explore
[`car::vif`](https://cran.r-project.org/web/packages/car/car.pdf#vif)
from Dr. John Fox (*emeritus*, McMaster University) *et al*.

## What is next?

We will explore interaction terms (Section 3.3.2) and ways to work
around problems that arise in linear regression models (Section 3.3.3).
