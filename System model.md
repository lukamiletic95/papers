### System model

Algorithms described in section four: `Algorithms`, rely on a network model explained here.



![](https://github.com/lukamiletic95/papers/blob/master/images/fig1.png)
<div align='center'> 
	<h4>Figure # - Network model</h4>
</div>

* *C - Client node*
* *FN - Full node*
* *V - Validator node*
* *CFG - Configuration file*

Let us consider a client-server architecture that consists of different types of nodes (*C, FN, V, CFG*). In a practical situation, a node may be represented by either a single hardware device or multiple hardware devices (e.g. within an organization). Therefore, this type of model representation allows for a certain level of abstraction that omits the hardware specification, yet enables a concise discussion without loss of generality.

It is assumed that a *C* node wants to have its **transaction** [^1] processed by the network. From the client's perspective, there are two important events that need to occur:
1. A transaction must be executed.
2. Evidence of that execution must be stored inside a blockchain.

> Every *V* node is also a *FN* node, b

To achieve that, a *C* node may connect to


[^1]: A transaction may represent any data that, when processed, is useful to the client (e.g. storing money transactions in a blockchain alongside executing them).


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MDU2NjM2NzksLTQ0MDY1ODU4OCwtMT
IxNjM4Nzk2NCw4MzU1OTYyMDAsMzE0MzUxNTQwLDE2OTQ0NjAy
NjcsNTI3ODI0OTU2LC05MTA1NDc1NzAsNjAwNTY4OTYxLC0xMD
U4NjE5MDczLDQ3MjEwNDk5MywxMTE1ODczNzMzLC0xMTA3Mzc4
NjAwLDQ3MDg3NjYzLC0xMjM4MDk1Mzk2LDk2MDEwNDM4OF19
-->