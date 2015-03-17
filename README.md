# kmeansStreamingHadoop
Lloyd's k-means algorithm written for MyHadoop on a SLURM batch scheduler

R files necessary to implement a bare-bones k-means clustering on a csv file.

Input must be a csv file with the continuous variables to be clustered, and an RData file with the initialized cluster centers.

kmeans.slurm gives an example batch submission script

TO DO:
initialization map-reduce run
summary map-reduce run to obtain:
  - boxplots for each variable by cluster
  - counts for each cluster
  - a subset of the data (perhaps 5k randomly selected observations)
