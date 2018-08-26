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

Concept where a three-phase gossiping is used is essential when there is a high network load, due to the fact that it guarantees that a message will not be delivered (added to the Mempool) more than once. **Furthermore, it enables that only a transaction's identifier is gossiped**. A need for sending an entire transaction happens only if that particular transaction identifier has been explicitly requested. This significantly reduces the amount of traffic that travels through the network.  

Those three phases are:

1. **PUSH →** represented by sending a *PROPOSE* message
2. **PULL →** represented by sending a *REQUEST* message
3. **PUSH →** represented by sending a *SERVE* message

Pseudocode is given (slightly modified version than the ones in [11] and [12]), followed by an explanation of the algorithm:

```go
// This is executed on each FN node

// Initialization
int f = ln(n);
Set<int> toPropose = EMPTY_SET;
Set<T> delivered = EMPTY_SET;
Set<int> requested = EMPTY_SET; 
start(GossipTimer(gossipPeriod));

// Phase 1 - PUSH T ids
func publish(T t) { // when C sends its transaction to FN
	bool valid = checkTx(transaction);
	if (valid == false) {
		return;
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
		if (!delivered.contains(t)) { // a message is delivered only once
			toPropose.add(t.id);
			deliverEvent(t);
		}
	}
}

// Miscellaneous
func selectNodes(int f) int {
	return f uniformly random nodes from the set of all nodes;
}

func gossip(Set<int> eventIds) {
	Set<Node> peerSubset = selectNodes(f);
	for (Node node : peerSubset) {
		send(PROPOSE, eventIds, node);
	}
}

func getEvent(int id) T {
	return T corresponding to the id;
}

func deliverEvent(T t) {
	delivered.add(t);
	addMempool(t);
}

```

<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig15.png" />
	<h4>Figure # - Messages exchanged during push-pull-push gossiping</h4>
</div>
<br/><br/>

The algorithm is based on exchanging three messages between two nodes, as shown in *Figure #*. Whenever a node wishes to broadcast information about transactions inside its Mempool, it sends a *PROPOSE* message (*PROPOSE* **contains only the transaction's id**). Afterwards, all the nodes who received that message can reply with a *REQUEST* (*REQUEST* **contains only the transaction's id**) message, in order to get a transaction corresponding to the requested id. Upon receiving a *SERVE* message (which is the **only message that contains the actual transaction and not only the id**), the requester may add the served transaction to its Mempool.

Since we assumed that a node has knowledge of the entire network, its fanout is initialized to *ln(n)*. This represents the size of the peer subset, or more precisely, the number of nodes that are going to receive the *PROPOSE* message. Node keeps track of three sets:

* *toPropose* - set of integers which contains the ids of transactions that will be proposed periodically by the *PROPOSE* message
* *delivered* - set of transactions T which contains all the transactions that were added to the Mempool
* *requested* - set of integers which contains the ids of transactions that were requested by the *REQUEST* message.

Due to the fact that the gossiping should occur periodically, in the initialization phase, a timer is started. Whenever a certain period of time (*gossipPeriod*) passes, a node sends information about every transaction that hasn't been proposed so far. After doing so, it empties its *toPropose* set. This way of operating is named *Infect and die*, because a node sends the information only once, and then "kills" that information.

Whenever a *C* node sends its transaction T to a *FN* node, function *publish()* is called. It first checks if the client's transaction is valid, then adds it to the Mempool and gossips its identifier.

When a *FN* node receives a *PROPOSE* message, it replies with a *REQUEST* message which contains only the requests for those transactions that haven't been requested before. Note that a node requests a transaction only once.

When a *FN* node receives a *REQUEST* message, it replies with a *SERVE* message which contains the actual transactions requested. Transactions are created via the call to *getEvent()* function.

When a *FN* node receives a *SERVE* message, it adds it to its Mempool, and marks it as a future transaction to-be-proposed, only if it already wasn't inside its Mempool.


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
eyJoaXN0b3J5IjpbMTM4MzM2NDUzOSwtMzg4MTUxNzcwLDE1Mz
E3NjM2MDQsLTY5NDkxMjM3OSwxNDA3NTk4NjQ5LC05MzUzNTg4
OTUsMTY2MjgzMzU5LC00NDA5MTczMjksLTE3OTg2ODI3MjUsMj
A5MjkyMzIzMiwtMTg3OTM1MjgxMiwxMDI5NjgwMjg3LDEyOTgw
OTM5NzRdfQ==
-->