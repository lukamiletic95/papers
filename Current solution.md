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
	
On the contrary, this

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc0NTM2Mjk1NiwtNjgzNzgxMTEzLDE1Nj
cyMTg0MjgsNjY3Nzk0MDU5LC0xODY5NDc1MzAyLC00MzMyMDI0
NzIsNDQ0OTg3MTU2LDExNTM3MDY0MjYsLTEyNjEzMTIzNjMsNT
I0MDMzNTA0LDE4OTY0MjQzNjgsLTExNjI3MzAwNjYsLTM5MzEy
NTMzMiwzMTM0NzEyNzRdfQ==
-->