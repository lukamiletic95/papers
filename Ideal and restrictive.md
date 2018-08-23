### Ideal and restrictive

The idea provided here solves the overhead problem that exists in the current solution. It does so by enabling a client transaction to be sent only to nodes that are inside the validator set (only *V* nodes in the current consensus instance will receive the transaction *T*). Be that as it may, it imposes a certain level of restrictiveness, owing to the fact that some additional assumptions have to be made (including the original ones from `System model - assumptions & summary`).

First of all, let us assume that the system now transitions between five states, as shown in *Figure #*.

<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm1/images/fig8.png" />
	<h4>Figure # - Five states of the system</h4>
</div>
<br/><br/>

The core idea of this solution is that a client can always find out IP addresses of nodes that will be in the validator set in the next consensus instance. When it does, it can send its transaction to all the nodes within that set. Even though a consensus instance may consist of multiple rounds (where ***k*** represents the number of those rounds) and each round may have a different proposer, if a *C* node sends its transaction to the entire validator set, it is **guaranteed** that *T* will be both executed and stored inside a blockchain, because a proposer is always chosen from the validator set.

The system loops through five different states:
1. **Determine validator set →** This is done by an external process. Upon determining the members of the validator set, *CFG* files of all *FN* nodes must be updated.

2. **Update CFG files →** Apart from containing information about the peer subset and connected clients, *CFG* file now contains information about the validator set that will be used in the next consensus instance. *CFG* files are updated in this state.

3. **Answer clients' requests →** It should be observed that a client may send its request completely asynchronously, when the system is in any of the five possible states. However, client will not be provided with information about the validator set until the system reaches this state.

4. **Timeout for clients' transactions →** Upon all the *C* that made a request being provided with the validator set to-be, the system will wait for a certain ***timeout***, so that all the *C* nodes can get the opportunity to send their transactions to appropriate *V* nodes.

5. **Consensus instance →** When all the transactions have been sent, a consensus instance may be initiated, so as to determine the next block of transactions within the blockchain.

<br/><br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm1/images/fig9.png" />
	<h4>Figure # - Client interaction with the system</h4>
</div>

* *C - Client node*
* *FN - Full node*
* *V - Validator node (is also a *FN* node)*
* *CFG - Configuration file*
<br/><br/>

*C* node can connect to a *FN* node of its own choice. That node could currently be a member of the validator set. Therefore, a *C* node can connect to either a *FN* or a *V* node. When it does, it requests IP addresses of the nodes in the validator set in the next consensus instance. Due to the fact that the system transitions between states, *C* node may have to wait  until the system reaches *state 3*, in order to receive a response. When that occurs, a *C* node has to send its transaction *T* to all of the nodes in the validator set. **Note that *C* sends *T* only once, and the validator set potentially changes at each blockchain height.** 

Let us consider a following scenario: 

	1. C node requests IP addresses of the validator set from a chosen FN node.
	2. C node receives the requested IP addresses.
	3. C node starts sending T to all the members of the validator set.
	4.* What if the size of some validator's Mempool exceeds the maximum number of transactions per block?

If the aforesaid scenario occurred, and the *FN* node added the transaction to its Mempool, thus exceeding the size of the block, it would be possible that the transaction would never be executed. There is a probability that the size of Mempools of all nodes in the validator set already equals maximum number of transactions per block, and that the added transaction will not be proposed in the following consensus instance. Since the validator set changes dynamically, none of the *FN* nodes in the current validator set may ever again appear in the validator set. Therefore, a client transaction could potentially get lost, due to the fact it had never been proposed.

To solve this, it is assumed that a *V* node remains in the validator set until its Mempool is emptied. A transaction is removed from a node's Mempool when it has become a part of the block to be added to the blockchain - meaning a consensus was reached on executing it. This guarantees that a transaction *T* will be executed  in one of the future consensus instances. It is also advisable that the algorithm for selecting a proposer is unbiased, and that it uniformly selects a node from the validator set.

```go

// Pseudocode for a C node

func send(T transaction, IPaddress providerIP) bool {
	Node provider = establishConnection(providerIP);

	if (provider == nil) {
		return false;
	}
	
	Set<Node> validatorSet = provider.requestValidatorSet(self);

	int numberReceived = 0;

	for (Node node : validatorSet) {
		bool success = node.receive(transaction, self);
		
		if (success == true) {
			numberReceived++;
		}
	}

	return numberReceived > 0;
}

```

At the beginning, a *C* node tries to establish a connection with a selected *FN* node, in order to obtain the validator set. In case the connection was not established successfully, the function returns false.  After that, a call of the *requestValidatorSet()* returns the validator set for the following consensus instance. Note that this is a blocking call, and the client awaits until it receives the information. Finally, *C* loops through all the members of the validator set and tries to provide them with the transaction *T*. In case the timeout expires or any other error occurs, the *receive()* function will return false. A counter variable *numberReceived* is used to denote the number of *V* nodes that have successfully received the clients transaction. If at least one *V* node had received *T*, it means that it will definitely be executed at some point in the future (due to the restriction regarding the validator set). However, the more members of the validator set receive *T*, the larger is the probability of it becoming proposed and executed earlier.

```go

// Pseudocode for a C node

CFGFile CFG;
Set<Node> requesters = ...;;

func main() {
	Set<Node> validatorSet = determineValidatorSet();
	
	CFG.setValidatorSet(validatorSet);

	for (No
}

func requestValidatorSet(Node requester) Set<Node> {
	requesters.add(requester);
	waitUntilReleased();
}

func receive(T transaction, Node sender) {

}

```



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIzNTY4MTc1MiwxNjEzOTExMjIxLDI1NT
U1ODY5NCwtMTcwMzYwNjIyNywtNzg0NDAwMDQ2LC00OTY5ODA2
MjMsLTEyMDkwMTYyMjksMTAwMTE2NTQ1OSwtMTc5OTU2MzI5Ni
wxNzI3NzY1NDE0LC01NzcwMTkyODAsMzg4NTQyNjQyLDYxNzIz
OTUzLC0xNzE5MzUzNTU3LDg0NDk0MDMwMSwtOTA4MzgzNzksLT
kyODg2NjMzOV19
-->