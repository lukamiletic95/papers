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

Concept where a three-phase gossiping is used is essential when there is a high network load, due to the fact that it guarantees that a message will not be delivered (added to the Mempool) more than once. Furthermore, it enables that, primarily, only a transaction's identifier is gossiped. A need for gossiping an entire transaction happens only if that particular transaction identifier has been explicitly requested. This significantly reduces the amount of traffic that travels through the network.  

Those three phases are:

1. **PUSH →** represented by sending a *PROPOSE* message
2. **PULL →** represented by sending a *REQUEST* message
3. **PUSH →** represented by sending a *SERVE* message

Pseudocode is given (slightly modified version than the ones in [11] and [12]), followed by an explanation of the algorithm:

```go

// Initialization
int f = ln(n);
Set<int> toPropose = EMPTY_SET;
Set<T> delivered = EMPTY_SET;
Set<int> requested = EMPTY_SET; 
start(GossipTimer(gossipPeriod));

// Phase 1 - PUSH T ids
func publish(T t) {
	if (sender == C) { 
		bool valid = checkTx(transaction);
		if (valid == false) {
			return;
		}
	}

	deliverEvent(t);
	gossip({t.id});
}

upon (GossipTimer % gossipPeriod) == 0 {
	gossip(toPropose);
	toPropose = EMPTY_SET; // Infect and die model
}

// Phase 2 - PULL wanted T ids
upon (receive(PROPOSE, proposed)) {
	Set<int> wanted = EMPTY_SET;
	for (int id : proposed) {
		if (!requested.contains(id)) {
			wanted.add(id);
		}
	}

	requested.add(wanted.getAll());
	reply(REQUEST, wanted);
}

// Phase 3 - PUSH requested T
upon (receive(REQUEST, wanted)) {
	Set<T> asked = EMPTY_SET;
	for (int id : wanted) {
		asked.add(getEvent(id));
	}

	reply(SERVE, asked);
}

upon (receive(SERVE, events)) {
	for (T t : events) {
		if (!delivered.contains(t)) {
			toPropose.add
		}
	}
}

```

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
eyJoaXN0b3J5IjpbODY1MTEyNzgzLC05MzUzNTg4OTUsMTY2Mj
gzMzU5LC00NDA5MTczMjksLTE3OTg2ODI3MjUsMjA5MjkyMzIz
MiwtMTg3OTM1MjgxMiwxMDI5NjgwMjg3LDEyOTgwOTM5NzRdfQ
==
-->