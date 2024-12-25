# Enron-POI-detection
 Identifying persons of interest through a simple ML based anomaly detection algorithm enhanced with communication network/graph structure.

$Y_{i \rightarrow j} = f_\theta \left( X_{i \rightarrow j} \right)$<br><br>
$\text{where, } f_\theta \text{ is the isolation forest model}$<br>
$\text{And, } Y_{i \rightarrow j} \text{ is the output(anomaly score) from the isolation forest model for an edge of the communication network with feature vector } X_{i \rightarrow j}$<br>

$Y_{i \rightarrow j}^{'} =  Y_{i \rightarrow j} \epsilon Y $
 
