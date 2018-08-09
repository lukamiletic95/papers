### About Tendermint

Tendermint ([1], [5]) is a Byzantine fault-tolerant blockchain. Its consensus algorithm, which is unique due to the fact it does not require mining, is used to order transactions within a blockchain in the presence of malicious actors (Byzantine generals problem - [6]). 

Tendermint consensus algorithm shows that unanimity can be achieved in an unreliable distributed system, under adversarial conditions.

The Tendermint network model is more complex than the one assumed in this paper (`System model - basics`) [^1]. Apart from *FN* nodes and *V* nodes, it also contains: 

* *Sentry* nodes, that are a special type of wrapper nodes to other *V* nodes. Sentry nodes establish a private connection with a certain *V* node. *V* nodes communicate among themselves via Sentry nodes. Therefore, Sentry nodes connected to different *V* nodes can be connected. The type of that connection can be a private, public or a secure VPN connection. 

* *Seed* nodes, which are useful when a new node becomes a part of the network - they provide it with a list of known peers.

Tendermint is a distributed network of nodes that have their own blockchain replicas in order to preserve the valid state of the system. In order for that to be able, Tendermint defines a constant ***f***, which represents a maximum value for the voting power of faulty processes in the network.

> Voting power is a number that is the property of each *V* node and it determines its "strength" when voting to reach a consensus. [1] - p. 5, III - TENDERMINT CONSENSUS ALGORITHM

Tendermint consensus algorithm guarantees successful termination, when the total voting power of nodes (marked as ***n***) of faulty processes does not exceed *3f*.

To summarize, Tendermint is a client-server platform which enables clients to execute their transactions, guarantees their proper completion and records them in a blockchain. It does so even if the network contains malicious nodes.

In `Section 3 - Definitions`, some of the important terms and ideas relevant to the algorithm discussion (`Section 4 - Algorithms`) will be explained alongside the system model used in this paper. Following, in the fourth section, a current solution along with the analysis of possible new solutions will be provided. This paper concludes in `Section 5 - Conclusion`.

[^1]: Detailed explanation on Tendermint node types can be found at: <https://github.com/tendermint/tendermint/blob/master/docs/spec/p2p/node.md>



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzODMxMDkzMSwxNDg1NjM5MjI3LDQ5MT
Q0MTA2OSwtMTc2OTI4NjM1NCw3MTYzNTI2MDUsMjk1NjAyNjQ4
LDIxNDcyNTgwMTEsLTE5MjE5NDM3MTgsLTE4OTU3NzMyOTUsLT
ExMTgzMjU2ODksMTA2NDQyMjU4MSwtNjYzNTYyMDA1LDY0NzA2
MTAzM119
-->