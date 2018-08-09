### Current solution

![](https://github.com/lukamiletic95/papers/blob/master/images/fig3.png)
<div align='center'> 
	<h4>Figure # - Mempool transaction gossiping in Tendermint</h4>
</div>

* *T - Transaction request*

Let us assume that a *C* node wants to have its transaction *T* processed by the Tendermint network, in a way explained earlier in the text.

Current Tendermint Mempool gossiping algorithm works as follows:

	1. When a FN receives a transaction T, it stores it inside its own Mempool.
	2. Then it broadcasts the transaction to all of the peers in its peer subset.
	3. 
	


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEyNzEyMDI0LDE4OTY0MjQzNjgsLTExNj
I3MzAwNjYsLTM5MzEyNTMzMiwzMTM0NzEyNzRdfQ==
-->