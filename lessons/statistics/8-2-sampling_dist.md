[Think Stats Chapter 8 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77) (scoring)

### Import packages
    import numpy as np
    import thinkstats2
    import thinkplot
    import matplotlib.pyplot as plt

### Standard Error function
    def RMSE(estimates, actual):
        """Computes the root mean squared error of a sequence of estimates.

        estimate: sequence of numbers
        actual: actual value

        returns: float RMSE
        """
        e2 = [(estimate-actual)**2 for estimate in estimates]
        mse = np.mean(e2)
        return np.sqrt(mse)

### Function to estimate standard error
    def Estimate82(n=10, iters=1000):
        lam = 2

        means = []
        medians = []
        for _ in range(iters):
            xs = np.random.exponential(1.0/lam, n)
            L = 1 / np.mean(xs)
            means.append(L)
    
        return means

### Loop where n = 10, 100, 1000 and store results in dictonary
    stderr_results = {}
    
    for i in range(1,4):
        n = 10**i
        means = Estimate82(n, iters=1000)
    
        cdf = thinkstats2.Cdf(means)
        thinkplot.Cdf(cdf)
        thinkplot.Config(xlabel='Sample mean', ylabel='CDF')
    
        stderr = RMSE(means, 2)
        ci = cdf.Percentile(5), cdf.Percentile(95)
        print(f'90% Confidence Interval: {ci}, Standard Error: {stderr}')
        stderr_results[n] = stderr

`90% Confidence Interval: (1.3123469275458386, 3.7049263194247826), Standard Error: 0.8050262243410099`  
`90% Confidence Interval: (1.7034858102186672, 2.359562824392451), Standard Error: 0.20107085610640935`  
`90% Confidence Interval: (1.9024770720204394, 2.105764088660618), Standard Error: 0.061375133002255944`

![plot 1](https://github.com/abalone23/dsp/blob/master/lessons/statistics/8-2a.png)

### Scatter Plot showing standard error decreasesing as n increases
  plt.scatter(list(stderr_results.keys()), list(stderr_results.values()))

![plot 2](https://github.com/abalone23/dsp/blob/master/lessons/statistics/8-2b.png)
