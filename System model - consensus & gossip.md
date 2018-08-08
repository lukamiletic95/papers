### System model - consensus & gossip

The following text explains part of the system model that enables the occurrence of events important to a client (1 and 2 in `System model → basics`).

It is assumed that, as a part of the system, there is a blockchain which is replicated on every *FN* node. This blockchain is shown in *Figure #*.

![](https://github.com/lukamiletic95/papers/blob/master/images/fig2.png)
<div align='center'> 
	<h4>Figure # - Blockchain inside <i>FN</i> nodes</h4>
</div>

The blockchain consists of blocks that are linked in some way (e.g. every block contains a hash function of the previous block). The first block in the blockchain is called a *Genesis* block, and every other block can trace its lineage back to it. In addition, the *Genesis* block also contains information about the first ever *validator set*. Each block inside the blockchain is described with a number - *height* (ordinal number of a block inside a blockchain). 

In order to add a new block to the blockchain, a set of nodes - ***validator set*** must reach a consensus for each *height* ***h***. A ***validator set*** is a set of *FN* nodes that is chosen for each consensus instance, which is executed whenever a new block at height *h* is to be added to the blockchain.

> Consensus instance represents one execution of the Tendermint consensus algorithm. Mainly, that algorithm will be considered a black-box in this paper, with the exception of revealing minor details of its implementation that are relevant to the functioning of the Mempool component and gossiping transactions from the Mempool. Consensus algorithm is described in detail in [1].

Therefore, the validator set potentially changes at each "blockchain height".

> The validator set is always chosen in a way to maximize the utilization of their voting power. [1] - p. 5, III - TENDERMINT CONSENSUS ALGORITHM

For that reason, there is a probability that the validator set will never change, for example in a network where there is a certain number of validators whose voting power dominates over others'. But in general, this must not be a presumption.

Each consensus instance consists of ***k*** rounds, where at each round a new ***proposer*** is potentially selected. 

> Once again, it is possible that through each round *1 ... k*, the proposer remains the same.

A proposer is a node that has the obligation to propose what the next block in the blockchain will be. It does so by selecting a certain number of transactions from its Mempool.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNTg4MTQxNzMsLTUzMTQ4NTQyNywxMj
M1ODUzNTY4LDQ1MDgxMjYxMywxMTQ1ODY2MTQ3LDIxNzc1MjI5
NCwtNDAyOTM1NzgyLDE4MTI4MjI4ODFdfQ==
-->