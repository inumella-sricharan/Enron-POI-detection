# Enron-POI-detection
 Identifying persons of interest through a simple ML based anomaly detection algorithm enhanced with communication network/graph structure.

$Y_{i \rightarrow j} = f_\theta \left( X_{i \rightarrow j} \right)$<br><br>
$\text{where, } f_\theta \text{ is the isolation forest model}$<br>
$\text{And, } Y_{i \rightarrow j} \text{ is the output(anomaly score) from the isolation forest model for an edge of the communication network with feature vector } X_{i \rightarrow j}$<br><br>


### <ins>shifting the anomaly scores</ins><br>

$Y_{i \rightarrow j}^{'} =  \[ \max \( Y_{i \rightarrow j} \hspace{0.15cm} \epsilon \hspace{0.15cm} Y) \] + 1$<br>
$\text{Here, } Y_{i \rightarrow j}^{'} \text{ is the shifted anomaly score}$<br>
$\text{where Y is the complete set of anomaly scores from }f_\theta$<br>
$\text{Here we are making negative anomaly scores into much higher positive scores. And the positive anomaly scores will be relatively lower positive among the transformed scores.}$<br>
$\text{Also we are making sure that the minimum of newly transformed score is 1 or more}$.<br>

### <ins>creating the transition matrix</ins><br>
Usually for pagerank algorithm the transition matrix is formed out of the adjacency matrix in which each entry indicates wether an edge exists from (node i --> node j) (1) or not (0).<br>
$P_{i \rightarrow j} = Y_{i \rightarrow j}^{'} \hspace{0.15cm} / \hspace{0.15cm} outdegree(i)$


 
