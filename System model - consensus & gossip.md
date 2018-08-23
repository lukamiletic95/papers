### System model - consensus & gossip

The following text explains part of the system model that enables the occurrence of events important to a client (1 and 2 in `System model - basics`).

It is assumed that, as a part of the system, there is a blockchain which is replicated on every *FN* node. This blockchain is shown in *Figure #*.

![](https://github.com/lukamiletic95/papers/blob/master/images/fig2.png)
<div align='center'> 
	<h4>Figure # - Blockchain inside <i>FN</i> nodes</h4>
</div>

The blockchain consists of blocks that are linked in some way (e.g. every block contains a hash function of the previous block). The first block in the blockchain is called a *Genesis* block, and every other block can trace its lineage back to it. In addition, the *Genesis* block also contains information about the first ever *validator set*. Each block inside the blockchain is described with a number - *height* (ordinal number of a block inside a blockchain).  Each block also contains a batch of transactions that are not only recorded permanently as a history of blockchain, but are also executed whenever a new block is added to the blockchain (**this satisfies 1 and 2 in** `System model - basics`).

In order to add a new block to the blockchain, a set of nodes - ***validator set*** must reach a consensus for each *blockchain height* ***h***. 

> Blockchain height is nothing but a height of a block to be added to the blockchain - it represents a number of blocks inside the blockchain, excluding the Genesis block.

A ***validator set*** is a set of *FN* nodes that is chosen for each consensus instance, which is executed whenever a new block at height *h* is to be added to the blockchain. Let the size of the validator set be denoted as ***n*** where ***n < m***. When a *FN* node is added to the validator set, it becomes a *V* node.

> Consensus instance represents one execution of the Tendermint consensus algorithm. Mainly, that algorithm will be considered a black-box in this paper, with the exception of revealing minor details of its implementation that are relevant to the functioning of the Mempool component and gossiping transactions from the Mempool. Consensus algorithm is described in detail in [1].

Therefore, the validator set potentially changes at each blockchain height. Validators are expected to be online, and the set of validators is curated by some external process. [^1]

> The validator set is always chosen in a way to maximize the utilization of their voting power. [1] - p. 5, III - TENDERMINT CONSENSUS ALGORITHM

For that reason, there is a probability that the validator set will never change, for example in a network where there is a certain number of full nodes whose voting power dominates over others'. But in general, this must not be a presumption.

Each consensus instance consists of ***k*** rounds, where at each round a new ***proposer*** is potentially selected from the validator set. 

> Once again, it is possible that through each round *1 ... k*, the proposer remains the same.

A ***proposer*** is a node that has the obligation to propose what the next block in the blockchain will be. It does so by selecting a certain number of transactions from its Mempool, creating a block, and then proposing it to the others in the validator set. If the proposition becomes accepted in that consensus round, then there will be no more rounds for that particular blockchain height due to the fact that the consensus was reached. New round is initiated whenever a consensus could not be reached in the previous round.

Hence, we come to a crucial conclusion regarding the Mempool and gossiping of transactions from it - **when a *FN* receives a *C* transaction inside its Mempool, it must gossip it in a way that guarantees (or at least provides a high probability outcome) that the transaction will eventually reach a Mempool of at least one *FN* node that will be a member of the validator set (hence, a *V* node) and a *proposer* in some round k<sub>i</sub> at some blockchain height h<sub>j</sub> in the future**. That particular course of events will then lead to a client transaction being both executed and stored within a blockchain.

[^1]: This information was extracted from Tendermint documentation: <https://github.com/tendermint/tendermint/wiki/Validators>. Algorithms for determining a validator set for each blockchain height is beyond the scope of this paper. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYxMzI3NDk1OSwzOTQ3MzAyNjUsNDYzMD
M2NDUzLDE3OTk4MjkwOTgsLTIxMTg0NTEwMiwxMTUwMDQwMzAw
LC0yMDQ0NDQyNzkxLDgwMzUxMjAzNSw4MTExNTQxMTIsMjkwNz
I2NjIzLC0xNjY3MTE4NjQ3LC04ODgzODMyMzUsLTExNzE0MDQx
ODgsMTY5MDM2NjgxOSwtMTA1ODgxNDE3MywtNTMxNDg1NDI3LD
EyMzU4NTM1NjgsNDUwODEyNjEzLDExNDU4NjYxNDcsMjE3NzUy
Mjk0XX0=
-->