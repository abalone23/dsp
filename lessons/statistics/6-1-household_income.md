[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

### Import packages
    import numpy as np
    import hinc

### Create DataFrame
    income_df = hinc.ReadData()

### Interpolate Sample function
    def InterpolateSample(df, log_upper=6.0):
        """Makes a sample of log10 household income.

        Assumes that log10 income is uniform in each range.

        df: DataFrame with columns income and freq
        log_upper: log10 of the assumed upper bound for the highest range

        returns: NumPy array of log10 household income
        """
        # compute the log10 of the upper bound for each range
        df['log_upper'] = np.log10(df.income)

        # get the lower bounds by shifting the upper bound and filling in
        # the first element
        df['log_lower'] = df.log_upper.shift(1)
        df.loc[0, 'log_lower'] = 3.0

        # plug in a value for the unknown upper bound of the highest range
        df.loc[41, 'log_upper'] = log_upper
    
        # use the freq column to generate the right number of values in
        # each range
        arrays = []
        for _, row in df.iterrows():
            vals = np.linspace(row.log_lower, row.log_upper, row.freq)
            arrays.append(vals)

        # collect the arrays into a single sample
        log_sample = np.concatenate(arrays)
        return log_sample

### Create sample with 1M upper bound
    log_sample = InterpolateSample(income_df, log_upper=6.0)
    sample = np.power(10, log_sample)

### Display initial results
    sample_median = np.median(sample)
    sample_mean = sample.mean()

    sample_median, sample_mean
`(51226.93306562372, 74278.7075311872)`

### Display Skewness
    # Raw moments are just sums of powers.
    def RawMoment(xs, k):
        return sum(x**k for x in xs) / len(xs)

    # The central moments are powers of distances from the mean.
    def CentralMoment(xs, k):
        mean = RawMoment(xs, 1)
        return sum((x - mean)**k for x in xs) / len(xs)

    # The standardized moments are ratios of central moments, with powers chosen to make the dimensions cancel.
    def StandardizedMoment(xs, k):
        var = CentralMoment(xs, 2)
        std = np.sqrt(var)
        return CentralMoment(xs, k) / std**k

    # The third standardized moment is skewness.
    def Skewness(xs):
        return StandardizedMoment(xs, 3)

    Skewness(sample)

`4.949920244429579`

### Pearson Skewness
    # Because the skewness is based on the third moment, it is not robust; that is, it depends strongly on a few outliers. Pearson's median skewness is more robust.
    def PearsonMedianSkewness(xs):
        median = np.median(xs)
        mean = RawMoment(xs, 1)
        var = CentralMoment(xs, 2)
        std = np.sqrt(var)
        gp = 3 * (mean - median) / std
        return gp

    PearsonMedianSkewness(sample)
    `0.7361105192428847`
