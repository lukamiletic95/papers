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
* Gossip communication protocol
* System model

<div align='center'> 
	<h3>Abstract</h3>
</div>

This paper will present a formal analysis of the Tendermint Mempool component. Tendermint is a blockchain that operates on a daily basis and is used worldwide. Blockchain schemes have gained popularity in recent years, as they provide decentralization alongside irrefutability.  

The aforementioned Mempool component is currently implemented in a way that it guarantees a certain outcome, yet it produces a large overhead. Herein, a theoretical analysis on gossip-based message passing will be provided, with the goal of reducing the overhead, but with the price of having only a probabilistic implementation of the Mempool component.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjMzMDQwNiwxMzE4NjI0NTEwLC05Mj
AxNDA4MDksMTIzODIyMDI4MSwtMTI3MDQyMTQ4Miw5Njk2MTY0
ODgsMTg2NjYwODUxOCwxNzcyMzE5Nzk1LDQ4MTMxOTU5Nyw3Mj
UyNTA0NTksLTEwMzg3NzMyMzcsLTEzOTYzNDE5NCwxNjk5MzQ5
NDgyXX0=
-->