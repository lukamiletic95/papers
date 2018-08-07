
Q&A:
- *References?*

Paper structure:
1. Abstract
2. Introduction
* Background (will shortly describe the whole idea for using blockchain nowadays)
* About Tendermint (will describe the Tendermint platform without attention to details)
3. Definitions
* Byzantine fault tolerance
* State Machine Replication
* Blockchain
* Mempool
* Gossip communication protocol
* System model

<div align='center'> 
	<h3>Abstract</h3>
</div>

This paper will present a formal analysis of the Tendermint Mempool component. Tendermint is a blockchain that operates on a daily basis and is used worldwide. Blockchain schemes have gained popularity in recent years, as they provide decentralization alongside irrefutability.  

The aforementioned Mempool component is currently implemented in a way that it guarantees a certain outcome, yet it produces a large overhead. It relies on a peer-to-peer gossip communication between nodes within a distributed network. 

Herein, a theoretical analysis on various gossip-based message passing algorithms will be provided, with the goal of finding the most suitable solution for reducing the overhead. However, reducing the overhead requires paying a price - algorithms cannot guarantee an outcome, they can only state that there is a high probability for an outcome to occur.

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzgxNjgyNzYwLC0xNDY2MDk2ODYzLC0xMj
YzMzA0MDYsMTMxODYyNDUxMCwtOTIwMTQwODA5LDEyMzgyMjAy
ODEsLTEyNzA0MjE0ODIsOTY5NjE2NDg4LDE4NjY2MDg1MTgsMT
c3MjMxOTc5NSw0ODEzMTk1OTcsNzI1MjUwNDU5LC0xMDM4Nzcz
MjM3LC0xMzk2MzQxOTQsMTY5OTM0OTQ4Ml19
-->