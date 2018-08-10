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

Until that moment arrives, the request is stored within the server's Mempool. Mempool is simply a RAM memory which holds requests that haven't been processed yet. When a consensus is reached and it is decided that a particular request may be processed (and therefore the state machine can make a transition into a new state), the request is removed from all of the Mempools.

Speaking in terms of blockchain, Mempool is a bottleneck of the system. It contains unconfirmed transactions, and the faster it is emptied, the better user experience it is to a client. In a situation where aserver's Mempool becomes overloaded, it may lead to that server crashing.

Therefore, it is vital to ensure "liveliness" of a Mempool, in the sense that it should constantly release processed transactions, in order to provide more space for the ones to come in the future. Speed of clearing a Mempool is tightly connected to a gossiping protocol used within the network.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NTM2OTMxNjcsLTg1MzUwMDkzNiwtMT
k5Mzk2MTc1NiwxNzYxMzM1ODY3LC04MTg5MzE3OTNdfQ==
-->