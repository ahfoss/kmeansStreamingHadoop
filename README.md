# kmeansStreamingHadoop
Lloyd's k-means algorithm written for MyHadoop on a SLURM batch scheduler

R files necessary to implement a bare-bones k-means clustering on a csv file.

Input must be a csv file with the continuous variables to be clustered, and an RData file with the initialized cluster centers.

NOTE: Variables must be normalized to variance 1. This ensures that the kmeans algorithm is not unduly influenced by any of the variables, and also ensures that a single EPSILON threshold can be sensibly used to assess convergence based on the mean vectors.

Centroids are initialized with random uniform draws from the [-3, 3]^p hypercube, where p gives the dimension of the data. Again, this is sensible only when the variables are normalized to variance 1, an additional reason why z-normalizing is critical.

A k-means run terminates when MAX\_NITER number of iterations elapses, or when \sum\_g^G ||\mu\_g^{(t)} - \mu\_g^{(t-1)}||\_1 < EPSILON, where G is the number of clusters and t is an integer denoting the current iteration.

If a cluster is found to be empty during a k-means run, it is re-initialized randomly.

kmeans.slurm gives an example batch submission script
km\_mapper.r gives the map step
km\_reducer.r gives the reduce step
km\_intermediary.r is executed in between successive map-reduce pairs
genData.r generates example data



TO DO:
initialization map-reduce run: standardize data to mean 0 and variance 1 and save original means and variances in a file in the summary directory.
summary map-reduce run to obtain:
  - boxplots for each variable by cluster
  - counts for each cluster
  - summary statistics (objective function value, within to between SS, etc)
Outer loop of repeated k-means runs with random initialized centroids

(1) num clust * X = 1.5 * num reducers

    EXAMPLE:
    10 clust, 20 reducers: X = 1.5*20/10 = 3 i.e. split into 30 chunks
    nlines = 100000: max chunk size = 3334 + a little

    calculate chunk size within cluster up-front
    feed chunk size into mapper
    a counter within the mapper creates the chunks of the given size
     + sorting quicker?
     - rounding errors for chunk size could lead to a very small chunk; should make sure chunk size is a bit too small

