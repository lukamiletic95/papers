# Formal analysis of the Tendermint Mempool protocol

Q&A:
- *References?*
- *Footnotes don't work on github only?*
- *Current solution : when a FN receives T, does it drop it if it is inside its Mempool, or does it re-gossip it? I understood that it drops T, but now I'm wondering if that is okay, because:*
  1. Node A receives T which is not inside its Mempool.
  2. Node A stores T inside its Mempool and gossips T.
  3. Node N connects to the network as a new full node, whose peer_subset = { A }.
  4. How will node N find out about T?

Paper structure:
1. Abstract
2. Introduction
* Background (will shortly describe the whole idea for using blockchain nowadays - describe blockchain here)
* About Tendermint (will describe the Tendermint platform without attention to details - describe BFT here)
3. Definitions
* State Machine Replication
* Mempool
* Gossip communication protocol
* System model - basics
* System model - consensus & gossip
* System model - assumptions & summary
4. Algorithms
* Current Solution
* ... my Solutions (includes analysis for each solution)
5. Conclusion
6. References
