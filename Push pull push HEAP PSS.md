### Push-pull-push HEAP with PSS

The idea of this algorithm combines a three-phase gossiping protocol described in [11], a protocol aware of network capabilities inequality - _**HEAP**_ (**HE**terogeneity-**A**ware-**P**rotocol)  [12], and a gossiping service for providing every node with its peer subset - the _**PSS**_ (**P**eer **S**ampling **S**ervice) [13]. Therefore, this algorithm consists of three parts, which are slightly modified in order to become applicable to our system model:

1. _Push-pull-push gossiping_
2. _HEAP_
3. _PSS_

Each of the three parts will be explained in further text.

This solution is based on building a dynamic, unstructured overlay across the network of nodes. Each node will change its peer subset dynamically, and gossip a client's transaction only to nodes in that peer subset. The size of that peer subset will be denoted as node's ***fanout*** - *f*.


#### 1. Push-pull-push gossiping

Let us consider a network of nodes, equivalent to that described in `System model`, with an additional assumption - each node in the network must have knowledge of every other node in the network. It is evident that by adopting this hypothesis, we suppose a full-mesh connectivity, which is not how the Tendermint network operates. However, this assumption is only adopted so that explaining of the first two parts of the algorithm would be simplified. In the third part - *PSS*, we will explain how this idea is applied to a wide area network with peer-to-peer connectivity.

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
	gossip({t.id}); // each T now has a unique identifier
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

The algorithm is based on exchanging three messages between two nodes, as shown in *Figure #*. Whenever a node wishes to broadcast information about transactions inside its Mempool, it sends a *PROPOSE* message (*PROPOSE* **contains only the transaction's id**). Afterwards, all the nodes who received that message can reply with a *REQUEST* message (*REQUEST* **contains only the transaction's id**), in order to get a transaction corresponding to the requested id. Upon receiving a *SERVE* message (which is the **only message that contains the actual transaction and not only the id**), the requester may add the served transaction to its Mempool.

Since we assumed that a node has knowledge of the entire network, its fanout is initialized to *ln(n)*. This represents the size of the peer subset, or more precisely, the number of nodes that are going to receive the *PROPOSE* message. Node keeps track of three sets:

* *toPropose* - set of integers which contains the ids of transactions that will be proposed periodically by the *PROPOSE* message
* *delivered* - set of transactions T which contains all the transactions that were added to the Mempool
* *requested* - set of integers which contains the ids of transactions that were requested by the *REQUEST* message.

Due to the fact that the gossiping should occur periodically, in the initialization phase, a timer is started. Whenever a certain period of time (*gossipPeriod*) passes, a node sends information about every transaction that hasn't been proposed so far. After doing so, it empties its *toPropose* set. This way of operating is named *Infect and die*, because a node sends the information only once, and then "kills" that information.

Whenever a *C* node sends its transaction T to a *FN* node, function *publish()* is called. It first checks if the client's transaction is valid, then adds it to the Mempool and gossips its identifier.

When a *FN* node receives a *PROPOSE* message, it replies with a *REQUEST* message which contains only the requests for those transactions that haven't been requested before. Note that a node requests a transaction only once.

When a *FN* node receives a *REQUEST* message, it replies with a *SERVE* message which contains the actual transactions requested. Transactions are created via the call to *getEvent()* function.

When a *FN* node receives a *SERVE* message, it adds T to its Mempool, and marks it as a future transaction to-be-proposed, only if it already wasn't inside its Mempool.

This push-pull-push approach reduces the payload in the network, owing to the fact that only a transaction's id is gossiped. Sending the actual transaction is delayed until *T* is explicitly required. 

However, this approach has one drawback. It assumes that every node in the network has a fanout *f = ln(n)*. This means that, when gossiping, every node will be equally burdened. Unfortunately, not all the nodes have the same capabilities. Therefore, imposing a high load on a node with lower capabilities may cause crashes, lower throughput and higher latency. HEAP, the second part of this solution, tends to solve this issue.

#### 2. HEAP

HEAP (HEterogeneity-Aware-Protocol) does not assume that the network is of homogeneous structure. It relies on nodes having different properties, in such a way that some nodes are faster and more productive than others. HEAP adapts a node's fanout according to its own bandwidth, average bandwidth, and the average fanout (which is, as we said, *ln(n)*).

Equation used in HEAP [12] is:

	myFanout = myBandwidth / averageBandwidth * averageFanout

*Average bandwidth* is updated according to the information about the capabilities of other nodes.

Updated pseudocode [12], which includes HEAP protocol, is given, followed by an explanation:

```go
// This is executed on each FN node

// Initialization
Set<Capability> capabilities = EMPTY_SET;
Bandwidth b = MY_BANDWIDTH; // my bandwidth
Bandwith _b_ = ...; // average bandwidth

int f = ln(n); // average fanout
Set<int> toPropose = EMPTY_SET;
Set<T> delivered = EMPTY_SET;
Set<int> requested = EMPTY_SET; 
start(GossipTimer(gossipPeriod));

start(AggregationTimer(aggregationPeriod));

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

// Aggregation protocol
upon (AggregationTimer % aggregationPeriod) == 0 {
	Set<Node> peerSubset = selectNodes(getFanout());
	for (Node node : peerSubset) {
		Set<Capability> node = capabilities.get(K);
		send(AGGREGATION, fresh, node);
	}
}

upon (receive(AGGREGATION, newCapabilities)) {
	capabilities.merge(newCapabilities);
	update(_b_, capabilities);
}

// Fanout adaptation
func getFanout() int {
	return b / _b_ * f;
}

// Miscellaneous
func selectNodes(int f) int {
	return f uniformly random nodes from the set of all nodes;
}

func gossip(Set<int> eventIds) {
	Set<Node> peerSubset = selectNodes(getFanout());
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
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig16.png" />
	<h4>Figure # - Additional HEAP messages</h4>
</div>
<br/><br/>

Note that a *FN* now has information about a set of capabilities. It uses this set to update the value of average bandwidth (*update(\_b\_, capabilities)*). Policy for updating the average bandwidth is implementation-dependent. Also, the data stored in *Capability* is implementation-dependent. It could be any data of interest to the network.

In HEAP, apart from exchanging *PROPOSE*, *REQUEST* and *SERVE* messages, nodes periodically exchange *AGGREGATION* messages, as shown in *Figure #*. Therefore, during the initialization phase, an *Aggregation timer* is started. On every *aggregationPeriod*, a *FN* will gossip information from its *Capability* set to a selected peer subset. Observe that the fanout is now calculated according to the aforementioned equation. When gossiping information about capabilities, a node will choose ***K*** values from the set. Value for *K* is implementation-dependent. Upon receiving an *AGGREGATION* message, a *FN* merges the new values with the existing ones and updates its *Capability* set accordingly.

The main role of HEAP is to optimize the utilization of nodes' capabilities (e.g. bandwidth), by adjusting their fanout in correspondence with the selected factors (*b, \_b\_, f*). Thus, nodes periodically gossip their information about other nodes' capabilities via *AGGREGATION* messages. This leads to nodes learning about the potential of the network and adjusting the information about that potential (*\_b\_*). That information is then used when calculating a node's fanout.

Finally, we will describe the third part of this solution, which enables it to be applied in  a wide area network. So far, it was assumed that a *FN* has knowledge of every other node in the network. To overcome this problem, we introduce the PSS (Peer Sampling Service) [13].


#### 3. PSS

The fundamental purpose of PSS is to provide every node with a *peer subset*. Until now, the presumption was that a node contains IP addresses of every other node in the network, and when it gossips information, it merely selects *f* nodes uniformly at random, hence creating the peer subset. However, maintaining information about every node in the network has a non-negligible overhead.

PSS is based on avoiding the aforesaid presumption. Its idea is to **gossip the membership information as well**. Therefore, this service can be considered **gossip based**. It creates a dynamic unstructured overlay across the network. Experimental analysis in [13] provides promising results when it comes to using PSS.

Note that when discussing this problem, we observe two aspects of gossip communication:
* **Peer sampling service** which provides every node with its peer subset - so far this was not gossip based, it was based on drawing a uniformly random sample from the set of all nodes.
* **Gossiping** messages of interest to that peer subset.

<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig17.png" />
	<h4>Figure # - Peer sampling service</h4>
</div>
<br/><br/>

* *D - Node descriptor*

As depicted in *Figure #* and described in [13], each *FN* node now contains information about its peer subset, which is in [13] denoted as a node's ***partial view***. *Partial view* is a list of ***node descriptors***, where each descriptor contains a node's ***IP address*** and a ***hop count***. *Hop count* represents the distance between two corresponding nodes expressed in the number of network hops between them. Partial view has at most one descriptor per node and is ordered according to increasing hop count. Each partial view is of the same maximum size, which will be denoted as ***c***.

PSS protocol contains two threads:
* **Active thread   →** which initiates communication with other nodes
* **Passive thread  →** which receives incoming messages

Pseudocode from [13] is given, slightly modified, and followed by an explanation:

```go
// Peer Sampling Service - pseudocode
bool push = ...; // at least one is always true
bool pull = ...;
PartialView myPartialView;

// Active thread
func activeThread() {
	while (true) {
		wait(SOME_TIME);
		Node p = selectPeer();

		if (push) {
			// 0 is the initial hop count
			Descriptor myDescriptor = new Descriptor(myIPAddress, 0);
			PartialView buffer = merge(myPartialView, myDescriptor);
			
			send(p, buffer);
		} 
		
		if (pull) {
			// make a pull request
			send(p, EMPTY_SET);
				
			PartialView receivedView = receiveViewFrom(p);		
			
			increaseHopCount(receivedView);
			PartialView buffer = merge(receivedView, myPartialView);
			myPartialView = selectView(buffer);			
		}
	}
}

// Passive thread
func passiveThread() {
	while (true) {
		{Node p, PartialView receivedView} = receiveMessage();
		
		// active thread of some other node has made a pull request
		if (receivedView == EMPTY_SET) {
			// 0 is the initial hop count
			Descriptor myDescriptor = new Descriptor(myIPAddress, 0);
			PartialView buffer = merge(myPartialView, myDescriptor);
			
			send(p, buffer);
		} else {
			increaseHopCount(receivedView);
			PartialView buffer = merge(receivedView, myPartialView);
			myPartialView = selectView(buffer);
		}
	}
}

```

The pseudocode is parametrized with two booleans - *push* and *pull*, as well as with two functions - *selectPeer()* and *selectView()*. Different strategies for determining these parameters are provided in [13] - p.5.

Function *selectPeer()* selects a peer to communicate with from the node's partial view.

Function *selectView()* truncates the *buffer* so as to make it of maximum length *c*.

Function *merge()* merges two views it receives as parameters. The resulting view is again ordered by hop count. If the resulting view contains two descriptors for the same IP address, only the one with the lower hop count is included.

Function *increaseHopCount()* increments the value of hop count in each descriptor of the view it received as a parameter.



#### Concluding the idea
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMTc3NzQ2LDEzNTM4Mjc5MjIsLTEwMD
k1OTU5NzQsNTA4MjgxMDY2LC0xMjE4MDgwODk3LC0xNTE2NTE3
MjQ2LC0xMTQ2NzQyMDcxLC0xMjAwNTYzOTkwLC0xMDUxMTE3Nz
Y1LC0xNjg2MzgzNDM1LDgzMDIyODM3Myw0NTMzNjk4MjEsLTcy
OTI2NTQ3NSwxMjgxMTcwODIxLDEwMDM4ODU1MDksMTA0NjgzND
g2MCwxNzM2NTQxMTcxLC0xMTM2NzczNTEwLC02ODk0NDc5MjQs
LTU4OTU0NzAyOF19
-->