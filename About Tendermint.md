### About Tendermint

Tendermint ([1], [5]) is a Byzantine fault-tolerant blockchain. Its consensus algorithm, which is unique due to the fact it does not require mining, is used to order transactions within a blockchain in the presence of malicious actors (Byzantine generals problem - [6]). 

Tendermint consensus algorithm shows that unanimity can be achieved in an unreliable distributed system, under adversarial conditions.

The Tendermint network model is more complex than the one assumed in this paper (`System model - basics`) [^1]. Apart from *FN* nodes and *V* nodes, it also contains: 

* *Sentry* nodes, that are a special type of wrapper nodes to other *V* nodes. Sentry nodes establish a private connection with a certain *V* node. *V* nodes communicate among themselves via Sentry nodes. Therefore, Sentry nodes connected to different *V* nodes can be connected. The type of that connection can be a private, public or a secure VPN connection. 

* *Seed* nodes, which are useful when a new node becomes a part of the network - they provide it with a list of known peers.

Tendermint is a distributed network of nodes that have their own blockchain replicas in order to preserve the valid state of the system. In order for that to be able, Tendermint defines a constant ***f***, which represents a maximum value for the voting power of faulty processes in the network.

[^1]: Detailed explanation on Tendermint node types can be found at: <https://github.com/tendermint/tendermint/blob/master/docs/spec/p2p/node.md>



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NjkyODYzNTQsNzE2MzUyNjA1LDI5NT
YwMjY0OCwyMTQ3MjU4MDExLC0xOTIxOTQzNzE4LC0xODk1Nzcz
Mjk1LC0xMTE4MzI1Njg5LDEwNjQ0MjI1ODEsLTY2MzU2MjAwNS
w2NDcwNjEwMzNdfQ==
-->