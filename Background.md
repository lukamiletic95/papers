### Background

In the modern era, which may as well be referred to as the *Data era*, collection of data has virtually become a part of everyday life. Nowadays, when nearly everyone is online, it is only natural that they expect to benefit from it as much as possible. In order for that to happen, various online services collect all sorts of data about their clients, with the goal of providing the highest quality service.

However, most of the information accumulated by different organizations is stored within those very organizations. Ergo, we may state that our private information is a part of a *single point of failure* system, meaning it could easily become compromised. What's more, there have been many security breaches which resulted in revealing one's personal data. This would be problem number one.

Problem number two would be a situation where someone could somehow deny a particular action (e.g. executing a bank transaction or signing a contract).

Those two particular problems cast doubt on the current model being used. There is always a third party organization that we completely confide in. Since this model has been proven to be unreliable, it raises a question: *"How to solve the aforesaid issues?"*.

So, in recent years, a new model has gained in popularity. This model is based on decentralization and indisputability. What if our private data could now be stored in multiple places (thus eliminating the *single point of failure* problem, along with it being completely resilient to modification by malicious third parties? As stated in [3], *it is easier to steal a cookie from a cookie jar, kept in a secluded place, than stealing the cookie from a cookie jar kept in a market place, being observed by thousands of people*.

This is where blockchain comes into picture. Blockchain drew attention in the past ten years, primarily due to its utilization within the Bitcoin transaction system [2]. Bitcoin is a cryptocurrency that relies on a distributed blockchain ledger which uses cryptography to achieve confidentiality as well as irrefutability. 

Blockchain is, in its essence, nothing more than a linked list of blocks which contain particular data [3]. Links are modeled using cryptographic hash functions. Since it is replicated on many devices within a distributed network, a consensus protocol must be defined, so that all the devices can unanimously decide what the next block will be. Blockchain is based on a *State machine replication approach*.

> State machine replication approach will b discussed later.

It is evident that blockchain became increasingly popular due to Bitcoin, however, it can be applied in a much more general way. It can be used not only for financial transactions, but also for any transactions that carry instructions, such as storing, querying and sharing data [4].

That particular idea, that a transaction in a blockchain can be pretty much anything, is used in Tendermint.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMTc2NzcxMzIsLTEzOTM1Njg3NDMsLT
IwOTE3Njk3NjQsLTY5Njc1MjYxMSwtMjA3MTE5MzY0NywtODQ1
NzQ5MzAzLDIxMzE5NDQyODUsLTE4OTExNDA3ODcsLTc2NTgyNz
I5MiwtODY5MTU2NjYxLC0xMzE5NDM0MTE5LC0yMTExNTU0MjUy
LC0xNDg2OTA5MTc3LC0xOTgyMjI3OTE1LC0zNTg5MjkzNzksMT
AxODU3NDQyNywtNDQ4NDg4NDIwXX0=
-->