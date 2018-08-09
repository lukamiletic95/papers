### Current solution

![](https://github.com/lukamiletic95/papers/blob/master/images/fig3.png)
<div align='center'> 
	<h4>Figure # - Mempool transaction gossiping in Tendermint</h4>
</div>

* *T - Transaction request*

Let us assume that a *C* node wants to have its transaction *T* processed by the Tendermint network, in a way explained earlier in the text.

Current Tendermint Mempool gossiping algorithm works as follows (this is executed on each *FN* node):

	1. FN receives T, either from a C, or from another FN.
	2. FN checks if T is already in its Mempool.
	3. If 2 == true, FN drops the message and does nothing.
	4. If 2 == false, FN stores T inside its Mempool.
	5. FN then gossips (broadcasts) T to everyone in its peer subset.
	
If we assume that the network (excluding *C* nodes) is modeled via a connected graph, then the number of messages that are exchanged for one transaction *T* is equal to *2 * num_of_vertices*, although the optimal number of messages is equal to the number of *FN* nodes decreased by 1 (*C* → *FN* message isn't taken into account since it is not a part of the gossiping protocol).
	
On the other hand, this algorithm guarantees that a message will reach everyone within the network, since we assumed that the minimum size of the peer subset is 1, and therefore there are no disconnected *FN* nodes.

**nisam siguran za ovo?**
However, Tendermint consensus algorithm is also gossip-based. Every time a proposer wishes to propose a block, it sends a special type of message - *PROPOSAL MESSAGE* [1]. That message contains a proposed block (which contains a batch of transactions), and is again gossiped to the peers of the round's selected proposer, and so forth. That may lead to some node receiving information about a transaction which it already received during the Mempool gossip phase, which is again a redundancy.

From all of the above, it may be concluded that a current solution used in Tendermint produces a large overhead, and that there is space for improving it.

> Simple improvement would be that, during the Mempool gossiping phase, a transaction *T* would not be broadcasted to a *FN* whom it was just received from.

Yet, this solution guarantees that the message will eventually be proposed and therefore added to the blockchain and executed.



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM0MjAyNTkyNSwxNTQ1MzgxOTM3XX0=
-->