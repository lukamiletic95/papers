### Push-pull-push HEAP with PSS

The idea of this algorithm combines a three-phase gossiping protocol described in [11], a protocol aware of network bandwidth inequality - _**HEAP**_ (**HE**terogeneity-**A**ware-**P**rotocol)  [12], and a gossiping service for providing every node with its peer subset - the _**PSS**_ (**P**eer **S**ampling **S**ervice) [13]. Therefore, this algorithm consists of three parts, which are slightly modified in order to become applicable to our system model:

1. _Push-pull-push gossiping_
2. _HEAP_
3. _PSS_

Each of the three parts will be explained in further text.

This solution is based on building a dynamic, unstructured overlay across the network of nodes. Each node will change its peer subset dynamically, and gossip a client's transaction only to a particular number of nodes in that peer subset. That number will be denoted as node's ***fanout*** - *f*.


#### 1. Push-pull-push gossiping

Let us consider a network of nodes, equivalent to that described in `System model`, with an additional assumption - each node in the network must have knowledge of every other node in the network. It is evident that by adopting this hypothesis, we suppose a full-mesh connectivity, which is not how the Tendermint network operates. However, this assumption is only adopted so that explaining of this part of the algorithm would be simplified. In the third part - *PSS*, we will explain how this idea is applied to a wide area network with peer-to-peer connectivity.

Hence, there exists a network of nodes where each node contains IP addresses of every other node. Basic gossiping idea is that a node, periodically, sends a message to a subset of nodes picked uniformly at random from the set of all nodes. As stated earlier, the size of that subset is denoted as *fanout*. Theoretical [9] and experimental [11] analysis has proven that in order to keep the network graph connected with high probability, optimal value for *f* is *ln(n)*, where *n* represents the number of nodes within the network.



<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig15.png" />
	<h4>Figure # - Messages exchanged during gossiping</h4>
</div>
<br/><br/>


#### 2. HEAP

<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig16.png" />
	<h4>Figure # - Additional HEAP messages</h4>
</div>
<br/><br/>


#### 3. PSS

<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig17.png" />
	<h4>Figure # - Peer sampling service</h4>
</div>
<br/><br/>

#### Concluding the idea
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM1MTM1ODM5NSwxNjYyODMzNTksLTQ0MD
kxNzMyOSwtMTc5ODY4MjcyNSwyMDkyOTIzMjMyLC0xODc5MzUy
ODEyLDEwMjk2ODAyODcsMTI5ODA5Mzk3NF19
-->