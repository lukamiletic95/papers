### Ideal and restrictive

The solution provided here solves the overhead problem that exists in the current one. It does so by enabling a client transaction to be sent only to nodes that are inside the validator set (only *V* nodes in the current consensus instance will receive the transaction *T*). Be that as it may, it imposes a certain level of restrictiveness, owing to the fact that some additional assumptions have to be made (including the original ones from `System model - assumptions & summary`).

First of all, let us assume that the system now transitions between five states, as shown in *Figure #*.

<br/><br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm1/images/fig8.png" />
	<h4>Figure # - Five states of the system</h4>
</div>

The core idea of this solution is that a client can always find out IP addresses of nodes that will be in the validator set in the next consensus instance. When it does, it can send its transaction to all the nodes within that set. Even though a consensus instance may consist of multiple rounds (where ***k*** represents the number of those rounds) and each round may have a different proposer, if a *C* node sends its transaction to the entire validator set, it is **guaranteed** that *T* will be both executed and stored inside a blockchain, because a proposer is always chosen from the validator set.

The system loops through five different states:
* **Determine validator set →** This is done by an external process. Upon determining the members of the validator set, *CFG* files of all *FN* nodes must be updated.

* **Update CFG files →** Apart from containing information about the peer subset and connected clients, *CFG* file now contains information about the validator set that will be used in the next consensus instance. *CFG* files are updated in this state.

* **Answer clients' requests →** It should be observed that a client may send its request completely asynchronously, when the system is in any of the five possible states. However, client will not be provided with information about the validator set until the system reaches this state.

* **Timeout for clients' transactions →** Upon all the *C* that made a request being provided with the validator set to-be, the system will wait for a certain *timeout*, so that all the *C* nodes can send their transactions to appropriate *V* nodes.

* **Consensus instance →** When all the transactions have been sent, a consensus instance may be initiated, so as to determine the next block of transactions within the blockchain.

<br/><br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm1/images/fig9.png" />
	<h4>Figure # - Client interaction with the system</h4>
</div>

* *C - Client node*
* *FN - Full node*
* *V - Validator node (is also a *FN* node)*
* *CFG - Configuration file*
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczNDM1MDQ3OCw2MTcyMzk1MywtMTcxOT
M1MzU1Nyw4NDQ5NDAzMDEsLTkwODM4Mzc5LC05Mjg4NjYzMzld
fQ==
-->