### Trees clustering

As mentioned earlier in the paper, nodes within the system model are not part of a single administrative domain. They are part of a large scale wide area network, where full connectivity cannot be established. For that reason, a *FN* node can only be connected to a subset of other *FN* nodes, which we denoted as the *peer subset*. Assumption number 9 from `System model - assumptions & summary` states that the minimal size of the peer subset is 1. Therefore, if a network were to be modeled by a graph, it would always be connected.

The idea behind this algorithm combines two of the solutions proposed in [9] - ***Flat membership server based protocol*** ([9] - 2.3.1) and ***Hierarchical membership protocol*** ([9] - 2.3.2). However, those two solutions applied directly to our system model would not improve its gossiping algorithm performances in a significant manner. Due to that, additional modification to the combination of the aforesaid solutions is proposed in further text.

 <br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm2/images/fig10.png" />
	<h4>Figure # - Dividing the network into clusters - <i>Hierarchical membership protocol</i></h4>
</div>
<br/><br/>

First and foremost, let us assume that the network is now divided into clusters. As presented in [9], nodes are clustered according to geographical proximity or some network-related metrics that refers to network proximity (e.g. round-trip delay or number of hops between nodes). [9] provides mathematical proof that only a small number of connections among clusters is required to keep the system connected. In *Figure #*, there are four clusters, *A*, *B*, *C* and *D*. There are also two types of connections:

* **Intercluster →** they represent links between different clusters.
* **Intracluster→** they represent links between nodes within the same cluster.

A *FN* node may be responsible for only one intercluster connection.

Since the number of intercluster connections doesn't have to be large, we have reduced the number of links within the network. In doing so, we have reduced the overhead that exists with the current solution. In the current solution, the number of messages that are exchanged when gossiping one client transaction *T* is equal to _2 * num_of_vertices_. If we reduce the number of vertices in the network graph, we reduce the number of messages exchanged during the gossip phase.

Furthermore, by clustering the nodes according to some criterion, we achieve having nodes that are in close "proximity" to one another, thus enabling a faster communication between them. Clustering reduces the latency in the network.

However, the overhead still exists within the clusters. There will still be redundant message passing when gossiping a transaction *T* from a node's Mempool. To solve that, following improvement is proposed:

 <br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm2/images/fig11.png" />
	<h4>Figure # - Tree clustering - <i>Flat membership server based protocol</i></h4>
</div>
<br/><br/>

The main culprit behind the overhead problem is - the graph. Graph can contain cycles. Since the network is modeled via a graph, with its links being bidirectional, a client's transaction can travel through the network using different paths, only to finally reach the same destination multiple times. To avoid that, a non-cyclical unidirectional data structure can be used - the tree.

If nodes within the cluster were to be connected in a tree-like scheme, it would result in each node receiving the message only once. However, two questions arise:

1. Who maintains the tree within the cluster?
2. How to propagate a message upward in a tree if we use a unidirectional link?

To answer the first question, an approach explained in [9], *Flat membership server based protocol*, can be applied. 

 <br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm2/images/fig12.png" />
	<h4>Figure # - State machine replication servers used for maintaining trees</i></h4>
</div>
<br/><br/>

We consider having a set of servers, where each server has its own state machine replica. Simply 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTExMjAyNTgyNCwyMDY4MzUzNTI2LC0xMj
c2OTIzODgzLDYzMTYyMDUwOF19
-->