# Enron-POI-detection
 Identifying persons of interest through a simple ML based anomaly detection algorithm enhanced with communication network/graph structure.

```math
\begin{align*}
Y_{i \rightarrow j} = f_\theta \left( X_{i \rightarrow j} \right) \\
where \\
a) Y_{i \rightarrow j} is the output from the isolation forest model for an edge of the communication network with feature vector X_{i \rightarrow j}
\begin{align*}
