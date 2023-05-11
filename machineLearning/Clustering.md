[[COMP24112]]

Clustering (Also known as cluster analysis) is the task of grouping a set of objects so that objects in the same group are more similar to each other than to those in other groups. We call each group a cluster

Clustering is unsupervised learning - there are no pre-defined classes and training data.

Okay but what is *similar?*
- The objects are usually characterised by a set of features corresponding to a set of data points in a high-dimensional space.
- Distance (or similarity) values are computed between pairs of data points, serving as a way to define similar

>[!example] Examples Include
>- Euclidean Distance
>- Cosine Similarity
>- Manhattan Distance
>- Minkowski Distance

Example:

![[Pasted image 20230424085112.png|600]]

case 1: (Have lungs)
case 2: (how produce offspring!)

Let's look at case 1 more in depth:

![[Pasted image 20230424085338.png|600]]

![[Pasted image 20230424085441.png|600]]

![[Pasted image 20230424085633.png|600]]

Key Tasks:
- Define an appropriate distance measure
- Identify the "natural" cluster number
- Find a way to group objects into "sensible" clusters. Different ways correspond to different clustering algorithms
- Assess whether the clustering output is a good one.

![[Pasted image 20230424085906.png|500]]

>3 clusters - this was very obvious in this case...

![[Pasted image 20230424085955.png|500]]

> Not so obvious anymore... and different algorithms will produce different clustering results.

![[Pasted image 20230424090203.png|500]]

Question, knowing the distance measure between two data points, how do we measure the distance between two data clusters?

##### Single-Link Measure
- it is the smallest distance between a data point in one cluster and a data point in the other cluster

![[Pasted image 20230424091520.png|500]]


##### Complete-Link Measure
- It is the largest distance between a data point in one cluster and a data point in the other cluster

![[Pasted image 20230424091847.png|500]]


##### Average-Link Measure
- it is the averaged distance between two data points in one cluster and data points in another cluster.
- 

![[Pasted image 20230424092053.png|500]]

##### Example

![[Pasted image 20230424092219.png|500]]


##### K-means clustering

- Simplest and most frequently used clustering algorithm.. and dates back to the 1960s - proposed by Hugo Steinhaus [1956] and James MacQueen used in [1967]

So how does it work?

![[Pasted image 20230424092830.png|500]]

So we first determine how many clusters we want to find, then we can use a distance measure to measure the dissimilarity between 2 points. And finally we need to set the initial cluster centres, so for instance:

`C1: (1, 0.7)` and `C2:(2, 0.7)`, which gives us the graph:

![[Pasted image 20230424093106.png|400]]

The first step is to measure that dissimilarity! And then the next step is to find the nearest cluster centre for each data point and assign it that cluster.

![[Pasted image 20230424093226.png|600]]

So `A` will belong to `C1`, and `B`, `C` and `D` will belong to `C2`

After we have done this:

![[Pasted image 20230424093530.png|500]]

So find a new centre for the cluster centre by averaging the points values in that cluster, and no we simply repeat steps 1 and 2, to update cluster membership.

![[Pasted image 20230424093739.png|500]]

We can stop the repeating of steps 1 to 3 when there is no change in membership of each cluster.

###### Summary:

![[Pasted image 20230424094005.png|600]]

![[Pasted image 20230424095209.png|600]]

The algorithm is sensitive to initial cluster centre points.
- Need to specify the number of clusters in advance
- Unable to handle noisy data and outliers
- Not suitable to discover for complex data patterns...

##### Agglomerative Algorithm - Hierarchical Clustering
- It groups objects into a tree of clusters - resulting in nested partitions layer by layer
- It partitions data sets sequentially with no need to know the cluster number in advance
- It includes two typical strategies
	- Agglomerative (bottom-up)
		- Start from treating each data point as a cluster
		- Then merge the atomic cluster into larger and larger clusters
	- Divisive (Top down)
		- Start from treating all data points as one single cluster
		- Then divide this cluster into smaller and smaller clusters.

![[Pasted image 20230424104141.png|500]]

On this course we will focus on the agglomerate algorithm, let's look at an example!

![[Pasted image 20230424104609.png|500]]

![[Pasted image 20230424104649.png|500]]![[Pasted image 20230424104805.png|500]]![[Pasted image 20230424105922.png|500]]![[Pasted image 20230424110150.png|500]]

##### Summary

![[Pasted image 20230424111123.png|600]]


### More clustering algorithms

- Centroid - represent a cluster by a center vector e.g K-means
- Hierarchical clustering - merge and divide e.g agglomerative clustering
- Distribution based -  assume objets from the same cluster should follow the same data distribution - good as the centre may not be a good representative!

![[Pasted image 20230424111400.png|300]]
- Density based - Define clusters as areas of higher density of data points

![[Pasted image 20230424111408.png|300]]




### How to validate whether the clustering results make sense... (Cluster Validation)

- Internal criteria (indexes) validates based on common sense without using external information
- External Criteria (indexes) validate against ground truth.

Normally a good clustering result should have a small within-cluster variance, while large between-cluster variance ($K$ is cluster number)

- Within-cluster variance (SSW) $$SSW = \sum^K _{i = 1} \sum_{p \in \text{cluster}_i} d^2(\text{x}_p, \text{c}_i )$$
- between-cluster variance (SSB) $$SSB = \sum^K _{i = 1} n_i\  d^2(\text{c}_i, \text{c} )$$
- F-ratio index $$F = K \frac{SSW}{SSB}$$
![[Pasted image 20230424112600.png|300]]

The above measures can be used to select cluster number (like a hyper-parameter!)


Another validation (External)
- Validate against a set of ground truth class labels of the data points
- Your cluster IDs and class labels give you two partitions

![[Pasted image 20230424112908.png|500]]


Challenges of comparing two partitions

- The clusters IDs in a partition can be assigned arbitrarily e.g a Permutation of the ID names still gives the same partition
- Two partitions may contain different numbers of clusters and classes
- How to find all possible correspondences between the clusters and classes.

##### Rand Index
- Rand index was proposed by William M Rand in 1971 
- It is computed by considering all pairs of data points by looking into agreement and disagreement against the ground truth
- It is defined based on the following agreements/disagreements table

![[Pasted image 20230424114946.png|600]]

- Rand is computed by: $$\text{RAND} = \frac{a+d}{a+b+c+d}$$

![[Pasted image 20230424120040.png|600]]


>[!info] Life time of clusters
>Get equation or something

