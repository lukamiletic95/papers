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

Let there be a set of ***m*** *full nodes*. Nodes in this model are not part of a single administrative domain. Consequently, a full-mesh connection among them cannot be administered. Due to that reason, a non-client node inside the network may only be connected to a subset of other *full nodes*. That subset is called a ***peer subset***.

> Every *V* node is also a *FN* node. However, vice-versa does not apply - *FN* node may or may not be a *V* node. In *Figure #*, it is assumed that every node marked as *FN* is not a *V* node.

Additionally, every *FN* node has a *CFG* file stored locally. This file contains information about the peer subset, connected clients, as well as about the ***validator set***.
> The term *validator set* will be explained in further text. 

*CFG* file may be configured statically (e.g. with IP addresses of clients, peers and validators), or it may change dynamically at runtime (e.g. when a client/peer connects/disconnects). 

It is assumed that a *C* node wants to have its **transaction** [^1] processed by the network. From the client's perspective, there are two important events that need to occur:
1. A transaction must be executed.
2. Evidence of that execution must be stored inside a blockchain.

To achieve that, a *C* node may connect to a *FN* node of its own choice. Upon establishing a connection with a *FN* node, a *C* node may send its transaction to the *FN* node. A client may, at any point in time, disconnect from a connected *FN* node, and connect to another one.

> It is assumed that each *FN* node has a public IP address, which enables a client to connect to it.

When a *FN* node receives a transaction, it stores it inside its own Mempool. In addition, the transaction is gossiped over the network to all of the peers of that particular *FN* node, so that it could be stored inside their own Mempools. Then the peers re-gossip the received transaction and so forth.

The following text explains part of the system model that enables the occurrence of events important to a client (1 and 2).

It is assumed that, as a part of the system, there is a blockchain which is replicated on every *FN* node. This blockchain is shown in *Figure #*.

![](https://github.com/lukamiletic95/papers/blob/master/images/fig2.png)
<div align='center'> 
	<h4>Figure # - Blockchain inside <i>FN</i> nodes</h4>
</div>

The blockchain consists of blocks that are linked in some way (e.g. every block contains a hash function of the previous block). The first block in the blockchain is called a *Genesis* block, and every other block can trace its lineage back to it. In addition, the *Genesis* block also contains information about the first ever *validator set*.



[^1]: A transaction may represent any data that, when processed, is useful to the client (e.g. storing money transactions in a blockchain alongside executing them).


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMjY2NDIxMzAsNjU1ODI3OTk3LC01Nj
I2NzAyMDAsLTE3NDIzOTY1MjgsMTMwMDQ2NDA4NywxNDM2OTI0
MjA4LDE3MDgwNTA1OSwtNDQwNjU4NTg4LC0xMjE2Mzg3OTY0LD
gzNTU5NjIwMCwzMTQzNTE1NDAsMTY5NDQ2MDI2Nyw1Mjc4MjQ5
NTYsLTkxMDU0NzU3MCw2MDA1Njg5NjEsLTEwNTg2MTkwNzMsND
cyMTA0OTkzLDExMTU4NzM3MzMsLTExMDczNzg2MDAsNDcwODc2
NjNdfQ==
-->