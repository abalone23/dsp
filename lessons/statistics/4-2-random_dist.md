[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

### Generate 1000 random (float) numbers between 0 and 1
rand_nums = np.random.random((1000,))

### Plot PMF (Probability Mass Function)
    rand_nums_pmf = thinkstats2.Pmf(rand_nums, label='rand_nums')
    thinkplot.Hist(rand_nums_pmf)
    thinkplot.Config(xlabel='Random Nums', ylabel='PMF')

### Plot CDF (Cumulative Distribution Function)
    rand_nums_cdf = thinkstats2.Cdf(rand_nums_pmf, label='rand_nums')
    thinkplot.Cdf(rand_nums_cdf)
    thinkplot.Config(xlabel='Random nums', ylabel='CDF', loc='upper left')
