### Trees clustering

As mentioned earlier in the paper, nodes within the system model are not part of a single administrative domain. They are part of a large scale wide area network, where full connectivity cannot be established. For that reason, a *FN* node can only be connected to a subset of other *FN* nodes, which we denoted as the *peer subset*. Assumption number 9 from `System model - assumptions & summary` states that the minimal size of the peer subset is 1. Therefore, if a network were to be modeled by a graph, it would always be connected.

The idea behind this algorithm combines two of the solutions proposed in [9] - ***Flat membership server based protocol*** ([9] - 2.3.1) and ***Hierarchical membership protocol*** ([9] - 2.3.2). However, those two solutions applied directly to our system model would not improve its gossiping algorithm performances in a significant manner. Due to that, additional modification to the combination of the aforesaid solutions is proposed in further text.

 <br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm2/images/fig10.png" />
	<h4>Figure # - Dividing the network into clusters</h4>
</div>
<br/><br/>

First and foremost, let us assume that the network is now divided into clusters. As presented in [9], nodes are clustered according to geographical proximity or some network-related metrics that refers to network proximity (e.g. round-trip delay or number of hops between nodes). [9] provides mathematical proof that only a small number of connections among clusters is required to keep the system connected. In *Figure #*, there are three clusters, *A*, *B* and *C*. There are also two types of connections:

* ***Interclus***
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NTg5NjA5MzQsNjMxNjIwNTA4XX0=
-->