### System model - assumptions & summary

To summarize the complete definition of the system model, in further text all of the assumptions made are listed:

* Full nodes have public IP addresses.
* Validator set is a subset of full nodes set - *n < m*.
* Validator set is a dynamic one - it changes potentially for each block to be added to the blockchain.
* The connection among the nodes in the network is not a full mesh, but a peer-to-peer connection.
* A *FN* may change its peer subset dynamically.
* A *C* may change its *FN* node dynamically.
* Information about clients and peers is stored within a *CFG* file.
* Network is non-fault-tolerant.

In the following sections, a current solution for gossiping transactions from a Mempool, which is a solution currently used in Tendermint, will be described. After that, various alternatives will be discussed and analyzed.
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTMyNjQzODMyLDM5ODc3ODQzN119
-->