# Enron-POI-detection
 Identifying persons of interest through a simple ML based anomaly detection algorithm enhanced with communication network/graph structure.

 

$Y_{i \rightarrow j} = f_\theta \left( X_{i \rightarrow j} \right)$ <br><br>
$\text{where, } f_\theta \text{ is the isolation forest model}$ <br>
$\text{And, } Y_{i \rightarrow j} \text{ is the output(anomaly score) from the isolation forest model for an edge of the communication network with feature vector } X_{i \rightarrow j}$ <br><br>


### <ins>Shifting the anomaly scores</ins> <br>

$Y_{i \rightarrow j}^{'} =  \[ \max \( \forall Y_{i \rightarrow j} \hspace{0.15cm} \epsilon \hspace{0.15cm} Y) \] + 1$ <br>
$\text{Here, } Y_{i \rightarrow j}^{'} \text{ is the shifted anomaly score}$ <br>
$\text{where Y is the complete set of anomaly scores from }f_\theta$ <br><br>
Here we are making negative anomaly scores into much higher positive scores. And the positive anomaly scores will be relatively lower positive among the transformed scores. <br>
Also we are making sure that the minimum of newly transformed score is 1 or more. <br>

### <ins>Creating the transition matrix</ins> <br>
Usually for pagerank algorithm the transition matrix is formed out of the adjacency matrix in which each entry indicates wether an edge exists from $node i \rightarrow node j$ (1) or not (0). <br>
The main aim while creating the transition matrix should be to keep the probabilities of transitioning towards anomalous nodes higher compared to those of normal nodes. i.e <br>

$P_{i \rightarrow j} \propto	Y_{i \rightarrow j}^{'}$ <br>
$\text{And after performing row-wise normalization,}$ <br>
$P_{i \rightarrow j} = Y_{i \rightarrow j}^{'} \hspace{0.5mm} / \hspace{0.5mm} outdegree(i)$ <br><br>

When we apply pagerank on the markov matrix (transpose of our transition matrix), because the probability of transitioning is more towards anomalous nodes, the flow of importance happens more towards anomalous nodes. And we end up discovering most important / influential nodes through the structure of the email-network graph. The advantage in this scenario is that the node with a high number of in-links simply does
not get ranked higher, the node having in-links with more anomalous nodes gets ranked higher(because we modified the probailities in the transition matrix to be more towards the anomalous nodes).




 
