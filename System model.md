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

> Every *V* node is also an *FN* node. However, vice-versa does not apply - *FN* may or may not be a *V* node. In *Figure #*, it is assumed that every node marked as *FN* is not a *V* node.

To achieve that, a *C* node may connect to


[^1]: A transaction may represent any data that, when processed, is useful to the client (e.g. storing money transactions in a blockchain alongside executing them).


<!--stackedit_data:
eyJoaXN0b3J5IjpbOTg3OTA4OTIyLC00NDA2NTg1ODgsLTEyMT
YzODc5NjQsODM1NTk2MjAwLDMxNDM1MTU0MCwxNjk0NDYwMjY3
LDUyNzgyNDk1NiwtOTEwNTQ3NTcwLDYwMDU2ODk2MSwtMTA1OD
YxOTA3Myw0NzIxMDQ5OTMsMTExNTg3MzczMywtMTEwNzM3ODYw
MCw0NzA4NzY2MywtMTIzODA5NTM5Niw5NjAxMDQzODhdfQ==
-->