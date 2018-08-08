### System model - consensus & gossip

The following text explains part of the system model that enables the occurrence of events important to a client (1 and 2 in `System model - basics`).

It is assumed that, as a part of the system, there is a blockchain which is replicated on every *FN* node. This blockchain is shown in *Figure #*.

![](https://github.com/lukamiletic95/papers/blob/master/images/fig2.png)
<div align='center'> 
	<h4>Figure # - Blockchain inside <i>FN</i> nodes</h4>
</div>

The blockchain consists of blocks that are linked in some way (e.g. every block contains a hash function of the previous block). The first block in the blockchain is called a *Genesis* block, and every other block can trace its lineage back to it. In addition, the *Genesis* block also contains information about the first ever *validator set*. Each block inside the blockchain is described with a number - *height* (ordinal number of a block inside a blockchain). 

In order to add a new block to the blockchain, a set of nodes - ***validator set*** must reach a consensus for each *height* ***h***. A ***validator set*** is a set of *FN* nodes that is chosen for each consensus instance, which is executed whenever a new block at height *h* is to be added to the blockchain.

> Consensus instance represents one execution of the Tendermint consensus algorithm. Mainly, 

Therefore, the validator set potentially changes at each "blockchain height"
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExODg3MzQ3OTUsMjE3NzUyMjk0LC00MD
I5MzU3ODIsMTgxMjgyMjg4MV19
-->