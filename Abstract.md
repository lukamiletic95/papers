
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
	<h1>Abstract</h1>
</div>

This paper will present a formal analysis of the Tendermint Mempool component. Tendermint is a blockchain that operates on a daily basis and is used worldwide. Blockchain schemes have gained popularity in recent years, as they provide decentralization alongside irrefutability.  

The aforementioned Mempool component is currently implemented in a way that it guarantees a certain outcome, yet it produces a large overhead. It relies on a peer-to-peer gossip communication between nodes within a distributed network. 

Herein, a theoretical analysis on various gossip-based message passing algorithms will be provided, with the goal of finding the most suitable solution to the overhead problem. However, achieving that requires paying a price - new algorithms cannot guarantee an outcome, they can only state that there is a high probability for an outcome to occur.

In addition, algorithms provided here are based on a network of nodes defined in the second section of this paper: `Definitions -> System model`. Although the network model is somewhat simplified  in comparison to the actual model used in Tendermint,
essential principles regarding the functioning of the Mempool  component remain the same. Furthermore, the ideas behind those algorithms are general and therefore could be modified, adjusted, and then practically implemented within the Tendermint network.

<u>**Keywords:**</u> Tendermint, Mempool, Blockchain, Gossip-based communication

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzk4MzYxNzE3LDk4NDgwMzI1MywtMTIxOT
c4MDE5NiwtMjA2NzYzOTQ0MiwtNDQ5NTk2OTMyLDE3MTg4NzE0
MTMsLTE3NDkwNDA3MDksLTE0NjYwOTY4NjMsLTEyNjMzMDQwNi
wxMzE4NjI0NTEwLC05MjAxNDA4MDksMTIzODIyMDI4MSwtMTI3
MDQyMTQ4Miw5Njk2MTY0ODgsMTg2NjYwODUxOCwxNzcyMzE5Nz
k1LDQ4MTMxOTU5Nyw3MjUyNTA0NTksLTEwMzg3NzMyMzcsLTEz
OTYzNDE5NF19
-->