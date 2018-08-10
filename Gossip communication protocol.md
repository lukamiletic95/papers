### Gossip communication protocol

We consider the same scenario described in the preceding subsection, only now, the main focus is on describing (in general), how a *C* request is able to reach all of the *S* in the network.

It should be noted that not all of the servers are interconnected, therefore, a full-mesh connection is not established within the assumed network. Due to that, for a client's request to reach all of the servers, the request must be transferred via some sort of a protocol that guarantees that, eventually, the information will reach everyone to whom it is of interest.

That protocol is a ***gossiping protocol***, which could simply be described as a network communication protocol used in large peer-to-peer distributed systems, for disseminating data among nodes within the network. 

> This paper primarily focuses on optimizing the current Mempool gossiping communication protocol.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQwMDM2MDI1NSwtMzM4MDcxOTYzXX0=
-->