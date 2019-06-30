[Think Stats Chapter 7 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2008.html#toc70) (weight vs. age)

### Import packages
    import first
    import pandas as pd

### Create DataFrame
    live, firsts, others = first.MakeFrames()
    live = live.dropna(subset=['agepreg', 'totalwgt_lb'])
    
### Create scatter plot
    thinkplot.Scatter(live['ager'], live['totalwgt_lb'], alpha=0.1, s=1)
    thinkplot.Config(xlabel='Age',
                     ylabel='Weight (lbs)',
                     legend=False)
### Use function from Think Stats and create scatter plot
    def BinnedPercentiles(df):
        """Bin the data by age and plot percentiles of weight for each bin.

        df: DataFrame
        """
        bins = np.arange(10, 48, 3)
        indices = np.digitize(df.ager, bins)
        groups = df.groupby(indices)

        ages = [group.ager.mean() for i, group in groups][1:-1]
        cdfs = [thinkstats2.Cdf(group.totalwgt_lb) for i, group in groups][1:-1]

        thinkplot.PrePlot(3)
        for percent in [75, 50, 25]:
            weights = [cdf.Percentile(percent) for cdf in cdfs]
            label = '%dth' % percent
            thinkplot.Plot(ages, weights, label=label)

        thinkplot.Scatter(live['ager'], live['totalwgt_lb'], alpha=0.1, s=1)
        thinkplot.Config(xlabel='Age', ylabel='Weight (%)', legend=True)
        
      BinnedPercentiles(live)
      
### Pearson's correlation is not robust in the presence of outliers, and it tends to underestimate the strength of non-linear relationships.
    https://stackoverflow.com/questions/3425439/why-does-corrcoef-return-a-matrix/3425548#3425548

    np.corrcoef(live['ager'], live['totalwgt_lb'])[1,0]
    `0.04004563293060176`
    
### Spearman's correlation is more robust, and it can handle non-linear relationships as long as they are monotonic.
    def SpearmanCorr(xs, ys):
        xs = pd.Series(xs)
        ys = pd.Series(ys)
        return xs.corr(ys, method='spearman')

    SpearmanCorr(live['ager'], live['totalwgt_lb'])
    `0.042058691482148566`
