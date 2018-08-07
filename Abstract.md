
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

Herein, a theoretical analysis on various gossip-based message passing algorithms will be provided, with the goal of finding the most suitable solution to the overhead problem. However, achieving that requires paying a price - new algorithms cannot guarantee an outcome, they can only state that there is a high probability for an outcome to occur.

In addition, algorithms provided here are based on a network of nodes defined in the second section of this paper - *Definitions -> System model*. Although the network model if somewhat simplified  in comparison to the actual model used in Tendermint, 

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTM4MTYxMDcsLTE0NjYwOTY4NjMsLTEyNj
MzMDQwNiwxMzE4NjI0NTEwLC05MjAxNDA4MDksMTIzODIyMDI4
MSwtMTI3MDQyMTQ4Miw5Njk2MTY0ODgsMTg2NjYwODUxOCwxNz
cyMzE5Nzk1LDQ4MTMxOTU5Nyw3MjUyNTA0NTksLTEwMzg3NzMy
MzcsLTEzOTYzNDE5NCwxNjk5MzQ5NDgyXX0=
-->