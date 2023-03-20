This folder contains implementatoin of similarity measures used for time series datasets.
Our goal is to find a way to understand if there is some commonality/similarity between source and target dataset.

## PAD - Proxy-A-Distance
### What is PAD?
Proxy-A-Distance (PAD) is a distance metric used for measuring the similarity between time series data. It is a relatively new method that is based on the concept of constructing a proxy series for each time series and then comparing the proxy series instead of the original time series.
The proxy series is constructed by taking the absolute differences between consecutive values in the original time series and then summing those differences. Once the proxy series is constructed for each time series, the PAD distance between the two time series is calculated as the Euclidean distance between their respective proxy series.

### How does the algorithm work?
```
A: 10, 11, 13, 12, 10, 9, 11, 14, 16, 18
B: 10, 11, 12, 11, 10, 11, 13, 16, 17, 19

For A,
|11-10| + |13-11| + |12-13| + |10-12| + |9-10| + |11-9| + |14-11| + |16-14| + |18-16|
= 1 + 2 + 1 + 2 + 1 + 2 + 3 + 2 + 2 = 16
For B,
|11-10| + |12-11| + |11-12| + |10-11| + |11-10| + |13-11| + |16-13| + |17-16| + |19-17|
= 1 + 1 + 1 + 1 + 1 + 2 + 3 + 1 + 2 = 13

sqrt((16 - 13)^2) = 3
```
***NOTE:For two time series dataset (source and target), we get the PAD values for each datapoint in both datasets and compare each datapoint from source dataset to each datapoint from target dataset, which creates a distance matrix. Lastly, we can take the mean of the distance matrix to get a final value for similarity.*** 

The PAD distance between A and B is 3, which indicates that the two time series are somewhat similar based on their trend and overall shape.

## DTW - Dynamic Time Warping
### What is DTW?
Dynamic Time Warping (DTW) is a technique used in time series analysis to measure the similarity between two time series that may vary in speed. DTW allows for non-linear alignment by warping the time axis of the two series. This means that the corresponding points between the two series are allowed to be shifted in time. 

### How does the algorithm work?
The basic idea of DTW is to find the best possible match between two time series by computing the distance between corresponding points. The algorithm works by creating a grid of all possible pairwise distances between the points of the two series, and then finding the optimal path through the grid that minimizes the total distance. The optimal path is found using dynamic programming, which allows for efficient computation of the optimal path.

### Example:
To understand how DTW works with different time steps, let's consider an example. Suppose we have two time series, A and B, with different time steps as shown below:
``` 
A: 1, 4, 5, 8, 9, 10
B: 2, 4, 7, 8
```
The first step in DTW is to create a matrix of pairwise distances between each point in series A and series B. The matrix will have a size of m x n, where m is the length of series A and n is the length of series B.
``` 
2   0   3   7
3   1   2   6
6   2   0   4
7   3   1   0
8   4   2   1
9   5   3   2 
```
The next step is to find the optimal path through the matrix that minimizes the total distance between the two series. The optimal path is found using dynamic programming, which computes the minimum distance path through the matrix from the starting point (1,1) to the ending point (m,n). ***The path can move only in the right, down, or diagonally-right-down directions.***    

The optimal path is shown as a thick line and corresponds to the non-linear alignment of the two time series. The total distance along this path is 2 + 1 + 0 + 0 + 1 + 2 = 6. This is the DTW distance between the two time series. 

## DBA - DTW Barycenter Averaging
### What is DBA ?
DBA (DTW Barycenter Averaging) is a method for computing a representative time series from a set of time series using Dynamic Time Warping (DTW) distance as a measure of similarity. The resulting representative time series, known as the DTW barycenter, is the time series that minimizes the sum of DTW distances to all the input time series.

***NOTE: More on Barycenter - The barycenter produced by the DBA algorithm is typically used as a representative time series for a set of time series. It captures the main features and trends of the input time series and can be used for tasks such as clustering, classification, and visualization.***

### How does the algorithm work?
The DBA algorithm works by iteratively computing the centroid of the aligned time series and recomputing the alignment to this centroid until convergence. Here are the main steps of the DBA algorithm:
```
X1 = [0, 1, 3, 4, 6]
X2 = [1, 2, 4, 5, 7]
X3 = [2, 3, 5, 6, 8]

```
1. Initialize the DTW barycenter as an arbitrary time series (or mean of all time series) from the input set.
```
Bary (B)= [1, 2, 4, 5, 7]
```
2. Align each input time series to the DTW barycenter using DTW distance.
```
DTW(X1, B) = 1.5
DTW(X2, B) = 0.5
DTW(X3, B) = 1.5

X1_aligned = [0, 1.5, 3, 4.5, 6]
X2_aligned = [1, 2, 4, 5, 7]
X3_aligned = [2, 3.5, 5, 6.5, 8]

```
3. Compute the centroid of the aligned time series.
```
B = [1.0, 2.0, 4.0, 5.0, 7.0]
```
4. Set the DTW barycenter to be the aligned centroid.
```
B = [1.0, 2.0, 4.0, 5.0, 7.0]
```
5. Repeat steps 2-4 until convergence.
The resulting DTW barycenter can be used as a representative time series for the input set, which can be useful for tasks such as clustering, classification, and visualization.
