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

A client *C* may send its request to a server, in order to receive a response. According to the *state machine replication* approach, this means that each server must receive this request and then process it. Yet, as shown in *Figure #*, a client *C* is connected to only one server *S*. In order for that request to reach other servers in the network, a protocol of some sort must be used. In Tendermint, the protocol being used is the ***gossip communication protocol***.

> Gossip communication protocol will be discussed in the following subsection.

For now, we assume that the request "somehow" reaches all servers. When it does, it is apparent that it cannot be processed instantly, not only because one server may currently be processing another request, but also due to the fact that a global consensus must be reached on what the next state of all the state machine replicas will be. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NzI3NTIxMTcsLTgxODkzMTc5M119
-->