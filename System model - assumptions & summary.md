### System model - assumptions & summary

To summarize the complete definition of the system model, in further text all of the assumptions made are listed:

1. Full nodes have public IP addresses.
2. Validator set is a subset of full nodes set - *n < m*.
3. Validator set is a dynamic one - it changes potentially for each block to be added to the blockchain.
4. The connection among the nodes in the network is not a full mesh, but a peer-to-peer connection.
5. A *FN* may change its peer subset dynamically.
6. A *C* may change its *FN* node dynamically.
7. Information about clients and peers is stored within a *CFG* file.
8. Network is non-fault-tolerant.
9. Minimum size of the peer subset is 1.

In the following sections, a current solution for gossiping transactions from a Mempool, which is a solution currently used in Tendermint, will be described. After that, various alternatives will be discussed and analyzed.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA0MjU4NTcxMCwtMzM3MjY0NzE4LDkzMj
Y0MzgzMiwzOTg3Nzg0MzddfQ==
-->