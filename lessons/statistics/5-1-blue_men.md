[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

### Import package
    import scipy.stats

### Heights are normally distributed so use scipy.stats.norm
    mu = 178
    sigma = 7.7
    dist = scipy.stats.norm(loc=mu, scale=sigma)

### Convert inches to centimeters
    height_lower = round(((5 * 12) + 10) * 2.54, 2)
    height_upper = round(((6 * 12) + 1) * 2.54, 2)
    height_lower, height_upper
`(177.8, 185.42)`

### Calculate
    dist.cdf(height_upper) - dist.cdf(height_lower)
`0.3427468376314737`

Note: This is about one standard deviation from the mean
