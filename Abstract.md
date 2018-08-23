<div align='center'> 
	<h1>Abstract</h1>
</div>

This paper will present a formal analysis of the Tendermint Mempool component. Tendermint is a blockchain that operates on a daily basis and is used worldwide. Blockchain schemes have gained popularity in recent years, as they provide decentralization alongside irrefutability.  

The aforementioned Mempool component is currently implemented in a way that it guarantees a certain outcome, yet it produces a large overhead. It relies on a peer-to-peer gossip communication between nodes within a distributed network. 

Herein, a theoretical analysis on various gossip-based message passing algorithms will be provided, with the goal of finding the most suitable solution to the overhead problem. However, achieving that requires paying a price - new algorithms cannot guarantee an outcome, they can only state that there is a high probability for an outcome to occur.

In addition, algorithms provided here are based on a network of nodes defined in the second section of this paper: `Definitions → System model`. Although the network model is somewhat simplified  in comparison to the actual model used in Tendermint,
essential principles regarding the functioning of the Mempool  component remain the same. Furthermore, the ideas behind those algorithms are general and therefore could be modified, adjusted, and then practically implemented within the Tendermint network.

<u>**Keywords:**</u> Tendermint, Mempool, Blockchain, Gossip-based communication

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NjM4NzQyMzcsMTg3MzM1NzE4MSw0ND
M2NTIwMTIsLTkzODIxMDU0Niw5ODQ4MDMyNTMsLTEyMTk3ODAx
OTYsLTIwNjc2Mzk0NDIsLTQ0OTU5NjkzMiwxNzE4ODcxNDEzLC
0xNzQ5MDQwNzA5LC0xNDY2MDk2ODYzLC0xMjYzMzA0MDYsMTMx
ODYyNDUxMCwtOTIwMTQwODA5LDEyMzgyMjAyODEsLTEyNzA0Mj
E0ODIsOTY5NjE2NDg4LDE4NjY2MDg1MTgsMTc3MjMxOTc5NSw0
ODEzMTk1OTddfQ==
-->