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
	
It is noticeable that the number of messages
	


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ1MDIzOTM1OSwtNDMzMjAyNDcyLDQ0ND
k4NzE1NiwxMTUzNzA2NDI2LC0xMjYxMzEyMzYzLDUyNDAzMzUw
NCwxODk2NDI0MzY4LC0xMTYyNzMwMDY2LC0zOTMxMjUzMzIsMz
EzNDcxMjc0XX0=
-->