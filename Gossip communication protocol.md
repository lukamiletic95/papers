### Gossip communication protocol

We consider the same scenario described in the preceding subsection, only now, the main focus is on describing (in general), how a *C* request is able to reach all of the *S* in the network.

It should be noted that not all of the servers are interconnected, therefore, a full-mesh connection is not established within the assumed network. Due to that, for a client's request to reach all of the servers, the request must be transferred via some sort of a protocol that guarantees that, eventually, the information will reach everyone to whom it is of interest.

That protocol is a ***gossiping protocol***, which could simply be described as a network communication protocol used in large peer-to-peer distributed systems, for disseminating data among nodes (servers) within the network [8]. 

Gossip communication protocol is based on (in most cases) randomly selecting a peer, and then gossiping (sending) some information to it. This can lead to a very quick spread of information, although it provides a certain redundancy - it is possible for a node in the network to receive a gossip message which contains data about something it already knows. The protocol can easily be compared to real life gossiping.

Although this protocol includes some overhead, it is reliable, scalable and easy to deploy. Furthermore, in case a peer crashes or its message gets lost in transport, that overhead would actually be useful - the receiving node would eventually get the message via some other peer.

General idea behind the gossiping protocol is described well in [9]. In that paper,  a pseudocode of a probabilistic algorithm is provided

``` java

System.out.println("Hello world!");

```

This paper primarily focuses on optimizing the current Mempool gossiping communication protocol. In the following subsections, a system model assumed herein will be explained.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzk2MzIwNTgzLDE1OTY4ODczMzAsLTEyMT
czODY0MTcsMTMzMTk5MjU1Nyw1NzIwMDExMDEsLTMzODA3MTk2
M119
-->