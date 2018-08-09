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
	
If we assume that the network (excluding *C* nodes) is modeled via a connected graph, then the number of messages that are exchanged for one transaction *T* is equal *2 * num_of_vertices*, although 
	


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMzOTg5ODUyNiw2Njc3OTQwNTksLTE4Nj
k0NzUzMDIsLTQzMzIwMjQ3Miw0NDQ5ODcxNTYsMTE1MzcwNjQy
NiwtMTI2MTMxMjM2Myw1MjQwMzM1MDQsMTg5NjQyNDM2OCwtMT
E2MjczMDA2NiwtMzkzMTI1MzMyLDMxMzQ3MTI3NF19
-->