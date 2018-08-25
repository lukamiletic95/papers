### Trees clustering

As mentioned earlier in the paper, nodes within the system model are not part of a single administrative domain. They are part of a large scale wide area network, where full connectivity cannot be established. For that reason, a *FN* node can only be connected to a subset of other *FN* nodes, which we denoted as the *peer subset*. Assumption number 9 from `System model - assumptions & summary` states that the minimal size of the peer subset is 1. Therefore, if a network were to be modeled by a graph, it would always be connected.

The idea behind this algorithm combines two of the solutions proposed in [9] - ***Flat membership server based protocol*** ([9] - 2.3.1) and ***Hierarchical membership protocol*** ([9] - 2.3.2). However, those two solutions applied directly to our system model would not improve its gossiping algorithm performances in a significant manner. Due to that, additional modification to the combination of the aforesaid solutions is proposed in further text.

 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc0MzQ2MDY4MCw2MzE2MjA1MDhdfQ==
-->