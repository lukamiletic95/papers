### System model

Algorithms described in section four: `Algorithms`, rely on a network model explained here.



![](https://github.com/lukamiletic95/papers/blob/master/images/fig1.png)
<div align='center'> 
	<h4>Figure # - Network model</h4>
</div>

* *C - Client node*
* *FN - Full node*
* *V - Validator node (is also a *FN* node)*
* *CFG - Configuration file*

Let us consider a client-server architecture that consists of different types of nodes (*C, FN, V, CFG*). In a practical situation, a node may be represented by either a single hardware device or multiple hardware devices (e.g. within an organization). Therefore, this type of model representation allows for a certain level of abstraction that omits the hardware specification, yet enables a concise discussion without loss of generality.

Let there be a set of ***m*** *full nodes*. Processes in this model are not part of a single administrative domain. Consequently, a full-mesh connection among them cannot be administered. Due to that reason, a non-client node inside the network may only be connected to a subset of other *full nodes*. That subset is called a ***peer subset***.

> Every *V* node is also a *FN* node. However, vice-versa does not apply - *FN* node may or may not be a *V* node. In *Figure #*, it is assumed that every node marked as *FN* is not a *V* node.

It is assumed that a *C* node wants to have its **transaction** [^1] processed by the network. From the client's perspective, there are two important events that need to occur:
1. A transaction must be executed.
2. Evidence of that execution must be stored inside a blockchain.

To achieve that, a *C* node may connect to a *FN* node of its own choice. Upon establishing a connection with a *FN* node, a *C* node may send its transaction to the *FN* node. A client may, at any point in time, disconnect from a connected *FN* node, and connect to another one.

> It is assumed that each *FN* node has a public IP address, which enables a client to connect to it.

When a *FN* node receives a transaction, it stores it inside its own Mempool. In addition, the transaction is gossiped over the network to all of the peers of that particular *FN* node, so that it could be stored inside their own Mempools. Then the peers re-gossip the received transaction and so forth.

[^1]: A transaction may represent any data that, when processed, is useful to the client (e.g. storing money transactions in a blockchain alongside executing them).


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMwMDQ2NDA4NywxNDM2OTI0MjA4LDE3MD
gwNTA1OSwtNDQwNjU4NTg4LC0xMjE2Mzg3OTY0LDgzNTU5NjIw
MCwzMTQzNTE1NDAsMTY5NDQ2MDI2Nyw1Mjc4MjQ5NTYsLTkxMD
U0NzU3MCw2MDA1Njg5NjEsLTEwNTg2MTkwNzMsNDcyMTA0OTkz
LDExMTU4NzM3MzMsLTExMDczNzg2MDAsNDcwODc2NjMsLTEyMz
gwOTUzOTYsOTYwMTA0Mzg4XX0=
-->