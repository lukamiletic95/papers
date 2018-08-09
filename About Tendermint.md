### About Tendermint

Tendermint ([1], [5]) is a Byzantine fault-tolerant blockchain. Its consensus algorithm, which is unique due to the fact it does not require mining, is used to order transactions within a blockchain in the presence of malicious actors (Byzantine generals problem - [6]). 

Tendermint consensus algorithm shows that unanimity can be achieved in an unreliable distributed system, under adversarial conditions.

The Tendermint network model is more complex than the one assumed in this paper (`System model - basics`) [^1]. Apart from *FN* nodes and *V* nodes, it also contains: 
* *Sentry* nodes, that are a special type of wrapper nodes to other *V* nodes.
* *Seed* nodes, that 

[^1]: Detailed explanation on Tendermint node types can be found at: <https://github.com/tendermint/tendermint/blob/master/docs/spec/p2p/node.md>



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzE5MjY0MzgsMjk1NjAyNjQ4LDIxND
cyNTgwMTEsLTE5MjE5NDM3MTgsLTE4OTU3NzMyOTUsLTExMTgz
MjU2ODksMTA2NDQyMjU4MSwtNjYzNTYyMDA1LDY0NzA2MTAzM1
19
-->