# Enron-POI-detection
 Identifying persons of interest through a simple ML based anomaly detection algorithm enhanced with communication network/graph structure.

### **<ins>Intro:</ins>** <br> 
This project is about identifying anomalous activities within a communication network and to identify key persons/entities of interest that are causing these anomalous and potentially malicious events. <br> 

The straight forward approach would be to use a ML based algorithm with some set of crafted features(for entities) in the network to identify anomalies and go through their information manually to take a decision upon their malicious intent. <br> 

But real-life scenarios are much more complicated and nuanced, there are thousands of events occuring in a network and to identify key-players involved in a malicious activity requires the combination of machine learning techniques and the information contained within the structure of the network. <br>

### **<ins>Example:</ins>** <br> 
several mule accounts within a banking system are involved in funneling black money through the system from undeclared sources of income. 
Usually there is a central orchestrator taking care of 3 key steps : placement, layering and integration. while we can capture a handfull of malicious mule accounts, uncovering the entire set of mule accounts which were involved in the act is not always guaranteed. That is because we are limited by the features and data that we can use to train the machine learning model. 

If we can use the information from the few set of malicious mule accounts we had detected earlier and harness the structure of the network, there is a good chance we can uncover many other mule accounts involed in the act that were not previously noticed. <br>

### **<ins>Workflow Formulation:</ins>** <br>
I decided to take up the Enron dataset to perform POI(person of interest) detection task.
This dataset has emails from inboxes 150 people of the enron organization. For 150 people, there are two problems associated:
1) 150 data points is less for a ML based anomaly detection algorithm.
2) Looking at the communications at an email level revealed a lot of complexity in communication patterns. For example:<br>
   a) The same person can have both Enron email-id and a personal email-id.<br>
   b) There are email-ids belonging to non-human entities.<br>
   c) A single email is sent from one email-id to several email-ids at once.<br>

These details would be lost while forming feature vectors for only 150 people. So I thought its better to go ahead by considering email-ids as entities in the communication network.<br>

Also, "If email level communications are complex, what about pairs of emails ?" as combinations of emails would provide an even larger and richer dataset to work with. So the approach would be :<br>

1) Communication between two email-ids consitutes an edge within the communication network. we can craft a feature vector ($X_{i \rightarrow j}$) for every edge
in the network.
2) Use Isolation Forest algorithm to generate anomaly scores to each of these edges. The lesser the score, the more anomalous an edge is.
3) Harness the structure of the network to identify emails\nodes that are most influential in causing anomalous events.<br><br>

write about the features that are considered in this section.


### <ins>Isolation forest algorithm:</ins> <br>

briefly mention about the working of iso. forest algo.
insert the equation for calculating anomaly score from path length of the trees of iso forest algo.

$Y_{i \rightarrow j} = f_\theta \left( X_{i \rightarrow j} \right)$ <br><br>
$\text{where, } f_\theta \text{ is the isolation forest model}$ <br>
$\text{And, } Y_{i \rightarrow j} \text{ is the output(anomaly score) from the isolation forest model for an edge of the communication network with feature vector } X_{i \rightarrow j}$ <br><br>


### <ins>Shifting the anomaly scores:</ins> <br>

$Y_{i \rightarrow j}^{'} =  \[ \max \( \hspace{0.15cm} \forall \hspace{0.15cm} Y_{i \rightarrow j} \hspace{0.15cm} \epsilon \hspace{0.15cm} Y) - Y_{i \rightarrow j} \] + 1$ <br>
$\text{Here, } Y_{i \rightarrow j}^{'} \text{ is the shifted anomaly score}$ <br>
$\text{where Y is the complete set of anomaly scores from }f_\theta$ <br><br>
Here we are making negative anomaly scores into much higher positive scores. And the positive anomaly scores will be relatively lower positive among the transformed scores. <br>
Also we are making sure that the minimum of newly transformed score is 1 or more. <br>

### <ins>Creating the transition matrix:</ins> <br>
Usually for pagerank algorithm the transition matrix is formed out of the adjacency matrix in which each entry indicates wether an edge exists from $\text{node i} \rightarrow \text{node j}$ (1) or not (0). <br>
The main aim while creating the transition matrix in this particular scenario should be to keep the probabilities of transitioning towards anomalous nodes higher compared to those of normal nodes. i.e <br>

$P_{i \rightarrow j} \propto	Y_{i \rightarrow j}^{'}$ <br>
$\text{And after performing row-wise normalization,}$ <br>
$P_{i \rightarrow j} = Y_{i \rightarrow j}^{'} \hspace{0.5mm} / \hspace{0.5mm} outdegree(i)$ <br><br>

### <ins>Role of pagerank algorithm:</ins> <br>
When we apply pagerank on the markov matrix (transpose of our transition matrix), because the probability of transitioning is more towards anomalous nodes, the flow of importance happens more towards anomalous nodes. And we end up discovering most important / influential nodes through the structure of the email-network graph. The advantage in this scenario is that the node with a high number of in-links simply does
not get ranked higher, the node having in-links with more anomalous nodes gets ranked higher(because we modified the probailities in the transition matrix to be more towards the anomalous nodes).

### Results

attach the results here from both isolation forest and pagerank algorithm.




 
