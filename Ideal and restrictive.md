### Ideal and restrictive

The solution provided here solves the overhead problem that exists within the current one. It does so by enabling a client transaction to be sent only to nodes that are inside the validator set (only *V* nodes in the current consensus instance will receive the transaction *T*). Be that as it may, it imposes a certain level of restrictiveness, owing to the fact that some additional assumptions have to be made (including the original ones from `System model - assumptions & summary`).

First of all, let us assume that the system now transitions between five states, as shown in *Figure #*.

<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm1/images/fig8.png" />
	<h4>Figure # - Five states of the system</h4>
</div>

The core idea of this solution is that the client can always find out IP addresses of nodes that will be in the validator set in the next consensus instance. When it does, it can send its transaction to all the nodes in

<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm1/images/fig9.png" />
	<h4>Figure # - Client interaction with the system</h4>
</div>

* *C - Client node*
* *FN - Full node*
* *V - Validator node (is also a *FN* node)*
* *CFG - Configuration file*
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjkyNjE0NjEsODQ0OTQwMzAxLC05MDgzOD
M3OSwtOTI4ODY2MzM5XX0=
-->