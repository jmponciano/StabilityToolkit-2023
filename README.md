
<!-- README.md is generated from README.Rmd. Please edit that file -->

# StabilityToolkit

<!-- badges: start -->

<!-- badges: end -->

The goal of StabilityToolkit is to provide an array of statistical and
mathematical functions to estimate parameters of a series of dynamic
models consisting of a pair of difference equations that follow the
abundance of plasmid-free and plasmid-carrying cells under various
biological hypotheses. The package also includes functions to simulate
data under each model. Estimation is done via Maximum Likelihood.
Hierarchical stochastic models are also considered. Further information
can be found in the author
[papers](http://people.clas.ufl.edu/josemi/papers/) made in
collaboration with [Eva Top](http://www.thetoplab.org/).

## Installation

You can install the development version from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("javirudolph/StabilityToolkit")
```

Once the package is installed, you can start using it by loading it
with:

``` r
library(StabilityToolkit)
```

## Preamble

If you have questions and run into problems, please e-mail me at
<josemi@ufl.edu>. Mathematics and statistics are fantastic tools to
translate fundamental questions in biology into testable predictions.
It’s the best mechanism that we have in science to formulate relevant
biological questions in a way that these can be directly confronted with
the data sets at hand. If you have multiple tentative explanations for
your results and you speak even just a little of the language of math,
you have what you need to translate those tentative explanations into
statistical hypotheses that can all be simultaneously tested against
your data. So if you want to make the most of this package, you should
familiarize yourself with the models in the [Top
lab](http://www.thetoplab.org/). A lot of time and effort went into
writing the models in a way that they are easily understood. A great
place to start is the paper by [De Gelder et
al 2004](https://www.genetics.org/content/168/3/1131) and the model
section of the paper by [Ponciano et
al 2007](https://doi.org/10.1534/genetics.106.061937). There you will
find all the models used in this package. Reading those papers is a
pre-requisite to use this package smartly, and not just as a
plug-and-play tool. Even if you don’t fully understand all of the
statistical details, you won’t regret spending time on these papers. It
is my hope that after reading these you will have what you need to
de-bug your analyses.

## A brief description of what this package can do

Suppose we want to fit the segregation-selection (SS) model to estimate
the plasmid cost and the segregation rate, along with the initial
fraction of segregants.  Let's try to fit this model to the G1000 data set saved in the package.
you can do this with a single line of code:

``` r
simpleSSfit <- dynamic.fit(data1 = G1000, model = "SS")
```

If you want to fit the Horizontal Transfer model, simply do:

``` r
simpleHTfit <- dynamic.fit(data1=G1000, model = "HT")
```

Now, various types of biological complexities can be added to the bare
bones model. For example, if you want to fit the SS model but you would
like to assume that the cost parameter is negative and therefore, that
carrying the plasmid is actually a benefit, then you can do that simply
by modifying the default `true.cost` argument like this:

``` r
simpleSSfit <- dynamic.fit(data1=G1000,model="SS", true.cost=FALSE)
```

Here’s another type of practical thing you may want to do. Suppose you
have two data sets coming from two distinct stability assays, and want
to test if they are driven by the same biological parameters (same cost,
segregation rate and if horizontal transfer is present, same transfer
probability). Suppose that your second data set object name is
`trial.data2`. If you want to do this test with the SS model, just
write:

``` r
comb.SSfit <- dynamic.fit(data1=trial.data,model="SS", comb=TRUE,data2=trial.data2)
```

and if you want to test this idea with the Horizontal Transfer (HT)
model, just type

``` r
comb.HTfit <- dynamic.fit(data1=trial.data,model="HT", comb=TRUE,data2=trial.data2)
```

Finally, here’s something else you may want to do. Assume you have data
from a competition assay and from a stability assay and you want to
leverage the information in the competition assay to better estimate the
cost. This package comes with two sample data sets to that effect, one
is a stability assay, called `data.compet` and another called
`data.stabexp`. These two data sets come from an ancestral line, so cost
is expected to be true cost. You can check out these two data sets by
loading them:

``` r
data(compet)
compet

data(stabexp)
stabexp
```

Then, to jointly estimate the SS model parameters by using data from the
competition and from the stability assay, just type

``` r
add.compet.fit <- dynamic.fit(data1=stabexp,model="SS",add.compet.data=TRUE, data2=compet)
```
