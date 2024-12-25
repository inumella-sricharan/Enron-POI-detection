# Enron-POI-detection
 Identifying persons of interest through a simple ML based anomaly detection algorithm enhanced with communication network/graph structure.

This project is about identifying anomalous activities within a communication network and to identify key persons/entities of interest that are causing these anomalous and potentially malicious events. 

The straight forward approach would be to use a ML based algorithm with some set of crafted features(for entities) in the network to identify anomalies and go through their information manually to take a decision upon their malicious intent. 

But real-life scenarios are much more complicated and nuanced, there are thousands of events occuring in a communication network and to identify key-players involved in a malicious activity requires the combination of machine learning techniques and the information contained within the structure of the network.

So, I picked up the enron dataset to perform POI detection task.
This dataset has emails from 150 people of the enron organization. Now if i go about forming features for only 150 people, there are two problems associated with it :
a) 150 is too few data points for a ML based anomaly detection algorithm.
b) Looking at the communications at an email level reveals a lot more complexity in information and communication patterns which we will end up ignoring if we only consider the 150 people.

I got a thought that "If email level communications are complex, what about pairs of emails ?" as combination of emails would provide a much larger and richer dataset to work with. So if we consider emails as the entities/nodes in the communication network then I thought of the following approach :

1) communication between two emails consitutes an edge within the communication network. we can craft a feature vector ($X_{i \rightarrow j}$) for every edge
in the network.
2) Use Isolation forest to provide anomaly scores to each of these edges. The lesser the score, the more anomalous an edge is.
3) Finally harness the structure of the network to identify emails\nodes that are most influential in causing anomalous events. 


$Y_{i \rightarrow j} = f_\theta \left( X_{i \rightarrow j} \right)$ <br><br>
$\text{where, } f_\theta \text{ is the isolation forest model}$ <br>
$\text{And, } Y_{i \rightarrow j} \text{ is the output(anomaly score) from the isolation forest model for an edge of the communication network with feature vector } X_{i \rightarrow j}$ <br><br>


### <ins>Shifting the anomaly scores</ins> <br>

$Y_{i \rightarrow j}^{'} =  \[ \max \( \hspace{0.15cm} \forall \hspace{0.15cm} Y_{i \rightarrow j} \hspace{0.15cm} \epsilon \hspace{0.15cm} Y) - Y_{i \rightarrow j} \] + 1$ <br>
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




 
