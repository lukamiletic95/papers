### Current solution

![](https://github.com/lukamiletic95/papers/blob/master/images/fig3.png)
<div align='center'> 
	<h4>Figure # - Mempool transaction gossiping in Tendermint</h4>
</div>

* *C - Client node*
* *FN - Full node*
* *V - Validator node (is also a *FN* node)*
* *CFG - Configuration file*
* *T - Transaction request*

Let us assume that a *C* node wants to have its transaction *T* processed by the Tendermint network, in a way explained earlier in the text.

Current Tendermint Mempool gossiping algorithm works as follows (this is executed on each *FN* node):

	1. FN receives T, either from a C, or from another FN.
	2. FN checks if T is valid (only if the sender is a C node - in our model, in Tendermint - always).
	3. If 2 == false, FN drops the message and does nothing.
	4. If 2 == true, FN checks if T is already in its Mempool.
	5. If 4 == true, FN drops the message and does nothing.
	6. If 4 == false, FN stores T inside its Mempool.
	7. FN then gossips (broadcasts) T to everyone in its peer subset.

Note that in step 2, a validity check would not have to be performed if the transaction *T* was received from a *FN* node, since we assume that the network is non-fault-tolerant and therefore, peers can trust each other unconditionally. However, since Tendermint is a Byzantine fault-tolerant network, step 2 is always performed, due to the fact that the transaction could have been received from a malicious peer.

```go

// Mempool gossiping algorithm in Tendermint - pseudocode

func receive(T transaction, Node sender) {
	// Node = {C, FN, V}

	if (sender == C) { 
		// in Tendermint - this condition does not exist
	
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
	for (Node node : getPeerSubset()) {
		node.receive(transaction, self);
	}
}

```

* *C* node calls function *receive()* whenever it wants to have its transaction *T* processed by the Tendermint network. 
* Type *Node* contains all the necessary information about a particular Node, and that is implementation-dependent.
* Function *checkTx()* is a Tendermint built-in function that checks the validity of a transaction [^1].
* Function *checkMempool()* checks if a transaction *T* is already within a node's Mempool.
* Function *addMempool()* adds a transaction *T* to a node's Mempool.
* Function *getPeerSubset()* returns the peer subset of the node executing the *receive()* function.
	
If we assume that the network (excluding *C* nodes) is modeled via a connected graph, then the number of messages that are exchanged for one transaction *T* is equal to *2 * num_of_vertices*, although the optimal number of messages is equal to the number of *FN* nodes decreased by 1 (*C* → *FN* message isn't taken into account since it is not a part of the gossiping protocol).
	
On the other hand, this algorithm **guarantees** that a message will reach everyone within the network, since we assumed that the minimum size of the peer subset is 1, and therefore there are no disconnected *FN* nodes.

It should also be observed that this solution also **guarantees** a FIFO execution of all *C* transactions. If a *C* node sends multiple transactions, 

Furthermore, Tendermint consensus algorithm is also gossip-based. Every time a proposer wishes to propose a block, it sends a special type of message - *PROPOSAL MESSAGE* [1]. That message contains a proposed block (which contains a batch of transactions), and is again gossiped to the peers of the round's selected proposer, and so forth. That may lead to some node which is not a part of the current validator set receiving information about a transaction which it already received during the Mempool gossip phase, which is again a redundancy.

From all of the above, it may be concluded that a current solution used in Tendermint produces a large overhead, and that there is space for improving it.

Simple improvement would be that, during the Mempool gossiping phase, a transaction *T* would not be broadcasted to a *FN* whom it was just received from. The aforementioned *receive()* function would be changed accordingly:

```go

// ...
	addMempool(transaction);
	for (Node node : getPeerSubset()) {
		if (node == sender) {
			continue;
		}
		
		node.receive(transaction, self);
	}
//...

```

Yet, the current solution guarantees that the message will eventually be proposed and therefore added to the blockchain and executed.

[^1]: Implementation of *checkTx()* function can be found at: <https://github.com/tendermint/tendermint/blob/master/mempool/mempool.go>



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NzE2MzYxMTEsNTcxOTc4Mjk5LDIwND
M2NTA1OTYsLTEyMjQ4MzA5ODEsMTA3NTE0NTg0MiwxMzg4MjAx
MDksMTA5MTM5ODM3MSwxNzM2ODM0OTUzLC0xNDI1MDk1NDY5LD
EzNDIwMjU5MjUsMTU0NTM4MTkzN119
-->