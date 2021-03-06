[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

## The NSFG respondent variable NUMKDHH is defined as:
“Number of biological/adopted/related/legal children under age 18 in household”

### Import packages
import numpy as np

import nsfg

import first

import thinkstats2

import thinkplot

### Create DataFrame
resp = nsfg.ReadFemResp()

### Create Probability Mass Function (PMF)
pmf = thinkstats2.Pmf(resp.numkdhh, label='numkdhh')

### Make Plot
thinkplot.Pmf(pmf)

thinkplot.Config(xlabel='Number of children', ylabel='PMF')

![plot 1](https://github.com/abalone23/dsp/blob/master/lessons/statistics/3-1a.png)

## Define
def BiasPmf(pmf, label):
    new_pmf = pmf.Copy(label=label)

    for x, p in pmf.Items():
        new_pmf.Mult(x, x)
        
    new_pmf.Normalize()
    return new_pmf

biased = BiasPmf(pmf, label='biased')

### Plot biased and actual PMF
thinkplot.PrePlot(2)

thinkplot.Pmfs([pmf, biased])

thinkplot.Config(xlabel='Number of children', ylabel='PMF')

![plot 2](https://github.com/abalone23/dsp/blob/master/lessons/statistics/3-1b.png)

### Display biased and actual mean
pmf.Mean(), biased.Mean()

`(1.024205155043831, 2.403679100664282)`
