# Node2Vec_DBLP_citation_graph

## About DBLP Citation Graph 
The DBLP Citation graph contains papers as the nodes and the references made as an edge to the other paper nodes. In addition to this there are features for each paper like venue, keywords, field of study, etc. Using Node2Vec algorithm, the feature embeddings are retireved for the task of spectral clustering. For our problem we choose DBLP dataset, created by the Database Systems and Logic Programming research group at the University of Trier, Germany, contains information about various research papers, conferences and authors.
<br>
<p align="center">
Whole dataset as citation graph:<br>
Number of nodes: 6703290<br>
Number of edges: 375344782 <br>
Average degree: 12.1303<br>
Number of Distinctly connected Components: 269609
</p>
Some connected components of the Citation graph:
<p align="center">
<img src="https://raw.githubusercontent.com/tanishkasingh9/Node2Vec_DBLP_citation_graph/master/g.png">
  </p>
<br>
Degree Distribution of the Citation Network (degree vs count of nodes):
<p align="center">
<img src="https://raw.githubusercontent.com/tanishkasingh9/Node2Vec_DBLP_citation_graph/master/dd1.jpg" height="300">
</p>
## Node2Vec
Graph Networks have been used as a medium for representing data and the relationships between the data and the database. It provides a data structure equipped to model the real
world data and visualize it in a form that is innate to human understanding. It represents relationships as connections and objects as nodes, and there are standard techniques that have been built to describe graphs. We establish the concept of sensemaking in graphs as tasks involving prediction of the labels, and link prediction between two nodes of a given graph network. For predicting labels or classifying a node, we cluster a node with other nodes that have higher probability of having the same label. Some applications of classification task on graph network are predicting functional labels of proteins in a protein - protein interaction graph, and predicting political affiliations based on purchase history. Link prediction task can be used in recommendation systems and predicting real world friends. In this graph, each paper is represented as a node where an edge between nodes exists if a paper cites another paper. The network is sparse, proving to be both an advantage, to design discrete algorithms given the task at hand, and a disadvantage, harder to generalize sparse data for statistical learning. Any supervised learning will require a set of discriminating and informative domain specific features based on expert
knowledge, which becomes harder for such graphical representations.
<br>

We have used the node2vec algorithmic framework in order to learn continuous feature representations from our input graph. This is a step that facilitates a number of
downstream machine learning tasks. In our specific case, we have used it to aid the Skip-Gram algorithm, where the optimization problem can be modeled as:
<p align="center">
<a href="https://www.codecogs.com/eqnedit.php?latex=\small&space;max_f\&space;\Sigma_{u\in&space;v}log\&space;P(N_s(u)|f(u))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\large&space;max_f\&space;\Sigma_{u\in&space;v}log\&space;P(N_s(u)|f(u))" title="\large max_f\ \Sigma_{u\in v}log\ P(N_s(u)|f(u))" /></a>
</p>
where, <a href="https://www.codecogs.com/eqnedit.php?latex=\small&space;P(N_s(u)|f(u))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\small&space;P(N_s(u)|f(u))" title="\small P(N_s(u)|f(u))" /></a> is the likelihood of observing a node that lies in the neighborhood of node u and <i>f</i> is the d dimensional mapping of the graph to feature representation we are trying to learn. For every node u, <a href="https://www.codecogs.com/eqnedit.php?latex=\small&space;N_s(u)&space;\in&space;V" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\small&space;N_s(u)&space;\in&space;V" title="\small N_s(u) \in V" /></a> which is the set of all vertices (nodes) present in the graph. This relationship of being neighborhood of a node is assumed to be independent of all other nodes and its effect on the presence of this relationship. 
<br>
To smoothly interpolate between DFS and BFS search algorithms for creating neighborhood for a node, the framework defines a biased random walk algorithm using DFS and BFS both. A fixed length random walk is generated from transition probabilities as:
<p align="center">
  <a href="https://www.codecogs.com/eqnedit.php?latex=\large&space;P(m_i&space;=&space;c&space;|&space;m_{i-1}&space;=&space;v)&space;=&space;\begin{cases}&space;\frac{\pi_{vc}}{Z}&space;&&space;\text{if&space;}&space;(v,c)&space;\in&space;E\\&space;0&space;&\text{otherwise}&space;\end{cases}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\large&space;P(m_i&space;=&space;c&space;|&space;m_{i-1}&space;=&space;v)&space;=&space;\begin{cases}&space;\frac{\pi_{vc}}{Z}&space;&&space;\text{if&space;}&space;(v,c)&space;\in&space;E\\&space;0&space;&\text{otherwise}&space;\end{cases}" title="\large P(m_i = c | m_{i-1} = v) = \begin{cases} \frac{\pi_{vc}}{Z} & \text{if } (v,c) \in E\\ 0 &\text{otherwise} \end{cases}" /></a>
  </p>
The equation defines conditional probability for reaching node c (on ith step) from node v (i-1th step), using the transition probability <a href="https://www.codecogs.com/eqnedit.php?latex=\small&space;\pi_{vc}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\small&space;\pi_{vc}" title="\small \pi_{vc}" /></a> and normalizing constant Z. For every node pair the transition probability is defined on the basis of parameters p and q to guide the search function <a href="https://www.codecogs.com/eqnedit.php?latex=\small&space;\pi_{vc}&space;=&space;\alpha_{pq}(x,c)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\small&space;\pi_{vc}&space;=&space;\alpha_{pq}(x,c)" title="\small \pi_{vc} = \alpha_{pq}(x,c)" /></a>. Shortest distance between given nodes d, and parameters p (probability of immediately revisiting a node) and q (degree of exploration) is used to set this search bias function, 
<p align="center">
 <a href="https://www.codecogs.com/eqnedit.php?latex=\large&space;\alpha_{pq}(x,c)&space;=&space;\begin{cases}&space;\frac{1}{p}&space;&&space;\text{if&space;}&space;d_{xc}&space;=&space;0\\&space;1&space;&&space;\text{if&space;}&space;d_{xc}&space;=&space;1\\&space;\frac{1}{q}&space;&&space;\text{if&space;}&space;d_{xc}&space;=&space;2&space;\end{cases}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\large&space;\alpha_{pq}(x,c)&space;=&space;\begin{cases}&space;\frac{1}{p}&space;&&space;\text{if&space;}&space;d_{xc}&space;=&space;0\\&space;1&space;&&space;\text{if&space;}&space;d_{xc}&space;=&space;1\\&space;\frac{1}{q}&space;&&space;\text{if&space;}&space;d_{xc}&space;=&space;2&space;\end{cases}" title="\large \alpha_{pq}(x,c) = \begin{cases} \frac{1}{p} & \text{if } d_{xc} = 0\\ 1 & \text{if } d_{xc} = 1\\ \frac{1}{q} & \text{if } d_{xc} = 2 \end{cases}" /></a>
 </p>

## Spectral Clustering
After our network is aptly converted into nxd dimensional feature matrix from the skip gram, there are two tasks that can be performed. For performing clustering task, we perform eigen decomposition for finding eigen-vectors corresponding to k smallest eigen-values. Spectral Clustering is performed for dimension reduction followed by K-Means, creating k partitions in the graph based on some similarity metric. The clustering is optimized by minimizing the following objective function, 
<p align="center">
  <a href="https://www.codecogs.com/eqnedit.php?latex=\large&space;\begin{aligned}&space;\\&space;min_{H}&space;&&space;trace(H'LH)&space;\\&space;&&space;\text{&space;subject&space;to&space;}&space;H'LH&space;=&space;I&space;\end{aligned}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\large&space;\begin{aligned}&space;\\&space;min_{H}&space;&&space;trace(H'LH)&space;\\&space;&&space;\text{&space;subject&space;to&space;}&space;H'LH&space;=&space;I&space;\end{aligned}" title="\large \begin{aligned} \\ min_{H} & trace(H'LH) \\ & \text{ subject to } H'LH = I \end{aligned}" /></a>
 </p>
 
where L is the Laplacian matrix, H is the eigen-vector of the sparse laplacian matrix, and I is the identity matrix. As mentioned, the laplacian matrix of the citation network is very sparse, and calculating an inverse of L is very costly. Scipy.sparse uses ARPACK algorithm to efficiently pick second smallest eigen-value directly using linalg.eigsh even when the input matrix is sparse.

## Citation Graph Clustering Result

Given below are results of spectral clustering for k=2 to k=5:
<img src= "https://raw.githubusercontent.com/tanishkasingh9/Node2Vec_DBLP_citation_graph/master/cluster.png">
