### Mempool

Again, let us assume a standard client-server architecture, where each server has its own state machine replica. 

<br/>
<div align='center'> 
	<img src="https://github.com/lukamiletic95/papers/blob/master/images/fig7.png" />
	<h4>Figure # - Client-server architecture with Mempools</h4>
</div>

* *C - Client*
* *S - Server*
* *MP - Mempool (Memory Pool)*

A client *C* may send its request to a server, in order to receive a response. According to the *state machine replication* approach, this means that each server must receive this request and then process it.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMyMjcwNzk1OSwtODE4OTMxNzkzXX0=
-->