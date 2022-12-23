# K means clustering

## 简介

k-means聚类算法是源自于信号处理的一种**矢量量化(`vector quantization`)**的方法，旨在将n个观察只划分到k个集群当中。
在这k个集群当中，观察值依据最临近平均值（同一个集群中的）来进行划分集群。

通过这么划分，将数据空间（观察值）划分为[Voronoi cells](https://en.wikipedia.org/wiki/Voronoi_cell),

k-means聚类最小化簇内方差[squared euclidean distance,平均欧式距离](https://en.wikipedia.org/wiki/Squared_Euclidean_distance),
但不是常规的欧式距离，这将是更困难的[Weber problem,韦伯问题](https://en.wikipedia.org/wiki/Weber_problem).

!> 韦伯问题：[the mean optimizes squared errors,优化均值误差](https://en.wikipedia.org/wiki/Weber_problem).

## 描述

$$
\begin{equation}
  \mathop{\arg\min}\limits_{S} \ \ \sum_{i=1}^k \sum_{X \in S_i} 
  =
  \mathop{\arg}
\end{equation}
$$

$\mathop{\arg\min}\limits_{\theta}$

## kmeans有什么用

## kmeans的
