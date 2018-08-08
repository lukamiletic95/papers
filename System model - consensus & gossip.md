### System model - consensus & gossip

The following text explains part of the system model that enables the occurrence of events important to a client (1 and 2 in `System model → basics`).

It is assumed that, as a part of the system, there is a blockchain which is replicated on every *FN* node. This blockchain is shown in *Figure #*.

![](https://github.com/lukamiletic95/papers/blob/master/images/fig2.png)
<div align='center'> 
	<h4>Figure # - Blockchain inside <i>FN</i> nodes</h4>
</div>

The blockchain consists of blocks that are linked in some way (e.g. every block contains a hash function of the previous block). The first block in the blockchain is called a *Genesis* block, and every other block can trace its lineage back to it. In addition, the *Genesis* block also contains information about the first ever *validator set*. Each block inside the blockchain is described with a number - *height* (ordinal number of a block inside a blockchain).  Each block also contains a batch of transactions that are not only recorded permanently as a history of blockchain, but are also executed whenever a new block is added to the blockchain (**this satisfies 1 and 2 in** `System model → basics`).

In order to add a new block to the blockchain, a set of nodes - ***validator set*** must reach a consensus for each *blockchain height* ***h***. 

> Blockchain height is nothing but a height of a block to be added to the blockchain - it represents a number of blocks inside the blockchain, excluding the Genesis block.

A ***validator set*** is a set of *FN* nodes that is chosen for each consensus instance, which is executed whenever a new block at height *h* is to be added to the blockchain. Let the size of the validator set be denoted as ***n*** where ***n < m***. When a *FN* node is added to the validator set, it becomes a *V* node.

> Consensus instance represents one execution of the Tendermint consensus algorithm. Mainly, that algorithm will be considered a black-box in this paper, with the exception of revealing minor details of its implementation that are relevant to the functioning of the Mempool component and gossiping transactions from the Mempool. Consensus algorithm is described in detail in [1].

Therefore, the validator set potentially changes at each blockchain height.

> The validator set is always chosen in a way to maximize the utilization of their voting power. [1] - p. 5, III - TENDERMINT CONSENSUS ALGORITHM

For that reason, there is a probability that the validator set will never change, for example in a network where there is a certain number of full nodes whose voting power dominates over others'. But in general, this must not be a presumption.

Each consensus instance consists of ***k*** rounds, where at each round a new ***proposer*** is potentially selected from the validator set. 

> Once again, it is possible that through each round *1 ... k*, the proposer remains the same.

A proposer is a node that has the obligation to propose what the next block in the blockchain will be. It does so by selecting a certain number of transactions from its Mempool, creates a block, and then proposes it to the others in the validator set.

Therefore, we come to a crucial conclusion regarding the Mempool and gossiping of transactions from it - **when a *FN* receives a *C* transaction inside its Mempool, it must gossip it in a way that guarantees (or at least provides a high probability outcome) that the transaction will eventually reach a Mempool of at least one *V* node that is going to be a proposer in some round k<sub>i</sub> at some blockchain height h<sub>j</sub> in the future**. That particular course of events will then lead to a client transaction being both executed and stored within a blockchain.
<!--stackedit_data:
eyJoaXN0b3J5IjpbODAzNTEyMDM1LDgxMTE1NDExMiwyOTA3Mj
Y2MjMsLTE2NjcxMTg2NDcsLTg4ODM4MzIzNSwtMTE3MTQwNDE4
OCwxNjkwMzY2ODE5LC0xMDU4ODE0MTczLC01MzE0ODU0MjcsMT
IzNTg1MzU2OCw0NTA4MTI2MTMsMTE0NTg2NjE0NywyMTc3NTIy
OTQsLTQwMjkzNTc4MiwxODEyODIyODgxXX0=
-->