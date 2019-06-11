[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

# Using the variable totalwgt_lb, investigate whether first babies are lighter or heavier than others. Compute Cohen’s d to quantify the difference between the groups. How does it compare to the difference in pregnancy length?

According to [Trending Sideways](https://trendingsideways.com/the-cohens-d-formula), *Practical Engineering and Statistics Updates*, by Carter Bowles:

> Cohen’s d is one of the most common ways we measure the size of an effect.
> Cohen’s d is simply a measure of the distance between two means, measured in standard deviations.

Allen Downey uses the following Python function for Cohen's D:

```
def CohenEffectSize(group1, group2):
    """Computes Cohen's effect size for two groups.
    
    group1: Series or DataFrame
    group2: Series or DataFrame
    
    returns: float if the arguments are Series;
             Series if the arguments are DataFrames
    """
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)
    return d
```

First let's check the mean of group1 (first babies) as well as group2 (other babies):

```
mean_firsts = firsts.totalwgt_lb.mean()
mean_others = others.totalwgt_lb.mean()

(7.201094430437772, 7.325855614973262)
```

We can see that first born babies are on average a little more than .1 lbs lighter than other babies.

Next we will check the Cohen's D:

```
group_firsts = firsts.totalwgt_lb
group_others = others.totalwgt_lb
CohenEffectSize(group_firsts, group_others)

-0.088672927072602
```

The negative result indicates that the first born babies are lighter than other babies which corresponds with the previously calculated mean results. Had the groups been reversed, the Cohen's D value would be positive.