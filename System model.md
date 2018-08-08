### System model

Algorithms described in section four: `Algorithms`, rely on a network model explained here.



![](https://github.com/lukamiletic95/papers/blob/master/images/fig1.png)
<div align='center'> 
	<h4>Figure # - Network model</h4>
</div>

* *C - Client node*
* *FN - Full node*
* *V - Validator node (is also a *FN*)*
* *CFG - Configuration file*

Let us consider a client-server architecture that consists of different types of nodes (*C, FN, V, CFG*). In a practical situation, a node may be represented by either a single hardware device or multiple hardware devices (e.g. within an organization). Therefore, this type of model representation allows for a certain level of abstraction that omits the hardware specification, yet enables a concise discussion without loss of generality.

It is assumed that a *C* node wants to have its **transaction** [^1] processed by the network. From the client's perspective, there are two important events that need to occur:
1. A transaction must be executed.
2. Evidence of that execution must be stored inside a blockchain.

> Every *V* node is also a *FN* node. However, vice-versa does not apply - *FN* node may or may not be a *V* node. In *Figure #*, it is assumed that every node marked as *FN* is not a *V* node.

To achieve that, a *C* node may connect to a *FN* node of its own choice. Upon establishing a connection with a *FN* node, a *C* node may send its transaction to the *FN* node. A client may, at any point in time, disconnect from a connected *FN* node, and connect to another one.

> It is assumed that each *FN* node has a public IP address, which enables a client to connect to it.

When a *FN* node receives a transaction, it 

[^1]: A transaction may represent any data that, when processed, is useful to the client (e.g. storing money transactions in a blockchain alongside executing them).


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk5NTk5NzQ3NCwxNzA4MDUwNTksLTQ0MD
Y1ODU4OCwtMTIxNjM4Nzk2NCw4MzU1OTYyMDAsMzE0MzUxNTQw
LDE2OTQ0NjAyNjcsNTI3ODI0OTU2LC05MTA1NDc1NzAsNjAwNT
Y4OTYxLC0xMDU4NjE5MDczLDQ3MjEwNDk5MywxMTE1ODczNzMz
LC0xMTA3Mzc4NjAwLDQ3MDg3NjYzLC0xMjM4MDk1Mzk2LDk2MD
EwNDM4OF19
-->