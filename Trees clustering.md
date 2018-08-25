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

Node may be responsible for only one intercluster connection - this is also a restriction presented in [9].

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

We consider having a set of servers, where each server has its own state machine replica. When a new node wants to join the network, which can occur asynchronously and is therefore a dynamic process, it sends a request to a server set which is in charge of that node's most "proximate" cluster. As stated in `State machine replication` subsection, that node is completely unaware of the existence of multiple servers. As far as it is concerned, it contacts a single server with a request to join the cluster. It is the responsibility of all the servers in the set to reach a consensus on where in the tree will the new node be added. When that occurs, they all collectively transition to the next state. 

Apart from having to handle membership requests, servers must also keep the tree as efficient as possible (balanced, or even complete). 

Replicating servers eliminates the *single point of failure* problem. However, they have to be properly synchronized, and therefore a consensus algorithm executed on them should be carefully chosen.

To answer the second question, let us consider a situation presented in *Figure #*:

 <br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm2/images/fig13.png" />
	<h4>Figure # - Propagating the message when using trees</i></h4>
</div>
<br/><br/>


> First, we only consider the situation where there are no clusters.

When a node receives client's transaction *T*, it can easily propagate it down its subtree. However, how will the message reach the rest of the main tree?

To solve this problem, we assume that each node contains information about its parent (e.g. parent's IP address). Thus, when a *FN* shown in *Figure #* receives *T*, it can propagate it towards its parent, then that parent has to propagate it to both its parent and its descendants and so on recursively.

Principle can be adopted:
* Only when a node receives a message from a child or from a client, it has to propagate it to its own parent - upward gossiping. 
* The node always has to propagate the message down the tree - downward gossiping.

```go

// Pseudocode for a FN/V node - clusters not taken into consideration

Node parent = ...;
Set<Node> children = ...;

func receive(T transaction, Node sender) {
	if (sender == C) { 
		bool valid = checkTx(transaction);
		if (valid == false) {
			return;
		}
	}
	
	addMempool(transaction);
	
	if (sender == C || children.contains(sender)) {
		if (parent != nil) {
			parent.receive(transaction, self);
		}
	}

	for (Node child : children) {
		if (child == sender) {
			continue;
		}

		child.receive(transaction, self);
	}
}

```

Note that there is no need for calling the *checkMempool()* function before adding a transaction to the node's Mempool. This is due to the fact that a *FN* will always receive *T* only once. When not taking clusters into account, this solution provides ideal performances. However, if we take clusters into account, redundancy is present.

First, let us explain how that is true. If we consider having three clusters, *A*, *B* and *C*, as shown in *Figure #*, we see that the network is now again a graph, and a cyclic one. For example, the following intercluster links form a cycle:

	B → C
	C → B

So, if there was a client within the cluster *B*, and it sent its transaction to any *FN* within that cluster, the transaction would be gossiped all over the cluster locally, via intracluster links. It would also be gossiped to cluster *C* via the B → C link. Inside cluster C, message would be gossiped locally, but then again returned to cluster B via the C → B link. 

Therefore, to avoid adding a duplicate of a transaction into a node's Mempool, this time a call to *checkMempool()* is required:

```go

// Pseudocode for a FN/V node - clusters taken into consideration

Node parent = ...;
Set<Node> children = ...;
Node interclusterLink = ...;

func receive(T transaction, Node sender) {
	if (sender == C) { 
		bool valid = checkTx(transaction);
		if (valid == false) {
			return;
		}
	}

	bool isInMyMempool = checkMempool(transaction);
	if (isInMyMempool == true) {
		return;
	}
	
	addMempool(transaction);
	
	if (sender == C || children.contains(sender) || isFromAnotherCluster(sender)) {
		if (parent != nil) {
			parent.receive(transaction, self);
		}
	}

	for (Node child : children) {
		if (child == sender) {
			continue;
		}

		child.receive(transaction, self);
	}

	if (interclusterLink != nil) {
		interclusterLink.receive(transaction, self);
	}
}

```

Observe that in this case, upward gossiping is also required when the sender is from another cluster. The check is performed by a call to *isFromAnotherCluster()* function, which could, for example, check if the sender's IP address belongs to the receiver's cluster.

Also, a new node-local-variable is added - *interclusterLink*. Since intercluster links are unidirectional, it is assumed that this variable is set only for a node which can send a message to another cluster.

In this case, better performance is achieved owing to grouping of "proximate" nodes into clusters, and thus decreasing the latency of the system. However, a node may still receive redundant messages, due to the fact that the network graph can contain cycles.

To solve this, a generalization of the idea to use trees inside a cluster can be applied to the whole network:

 <br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm2/images/fig14.png" />
	<h4>Figure # - Tree clustering - final solution</i></h4>
</div>
<br/><br/>

Now all of the clusters that constitute the network form a tree. In this case, a node can be responsible for more than one intercluster connection - this is less restrictive than [9].

Root of every tree inside a cluster contains links (IP addresses) towards roots of trees inside its clusters children. The information is propagated in a completely same manner as in a in-cluster tree.

Observe that the restriction in [9] did not have to be mitigated for this algorithm to work. A node could still remain responsible for only one intercluster link, but this would result in multiple nodes within a cluster being connected to different clusters. Therefore, in further text, we assume that a only a root can contain intercluster connections.

```go

// Pseudocode for a FN/V node - root

Node parent = ...; // is always from another cluster
Set<Node> children = ...;
Set<Node> interclusterChildren = ...;

func receive(T transaction, Node sender) {
	if (sender == C) { 
		bool valid = checkTx(transaction);
		if (valid == false) {
			return;
		}
	}
	
	addMempool(transaction);
	
	if (parent != nil) {
		parent.receive(transaction, self);
	}

	for (Node child : children) {
		if (child == sender) {
			continue;
		}

		child.receive(transaction, self);
	}

	for (Node interclusterChild : interclusterChildren) {
		if (interclusterChild == sender) {
			continue;
		}
		
		interclusterChild.receive(transaction, self);
	}
}

```

```go

// Pseudocode for a FN/V node - non-root node

Node parent = ...; // is always from the same cluster
Set<Node> children = ...;

func receive(T transaction, Node sender) {
	if (sender == C) { 
		bool valid = checkTx(transaction);
		if (valid == false) {
			return;
		}
	}
	
	addMempool(transaction);
	
	if (sender == C || children.contains(sender)) {
			parent.receive(transaction, self); // parent is never nil for a non-root node
	}

	for (Node child : children) {
		if (child == sender) {
			continue;
		}

		child.receive(transaction, self);
	}
}

```

In this case, a call to *checkMempool()* is also unnecessary. Every *FN* will receive *T* only once.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ5MjIwMzI2MiwxNjQ4NTY4ODIyLC0xND
EwNDU3NjcyLC0xNTMyMDAxNjExLDg0MTExMjQ3OSwxNjA3MDkz
NjIxLC05Mzc3MTIwODUsLTE4Nzc0OTQ3MTgsLTUxNTczODg1Mi
wxMzk2NDk5MjE0LC02MTk4ODg3NTAsMTM3OTM1OTE1OCwyMDY4
MzUzNTI2LC0xMjc2OTIzODgzLDYzMTYyMDUwOF19
-->