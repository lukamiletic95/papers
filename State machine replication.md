### State machine replication

Let us assume a standard client-server architecture. Server provides a service for a client, and in order for a client to make use of that service, it has to send a request, and wait for a response.

<div align='center'> 
	<img src="https://github.com/lukamiletic95/papers/blob/master/images/fig4.png" />
	<h4>Figure # - Standard client-server architecture</h4>
</div>

* *C - Client*

Although this is the simplest approach in implementing a client-server service, its main disadvantage is in having only one server. In case that server becomes faulty, the whole service will be unavailable to a client. That one server may be considered a single point of failure.

Therefore, we assume another, more advanced and reliable client-server architecture, shown in *Figure #*, which is based on replicating servers using ***state machine*** approach.

![](https://github.com/lukamiletic95/papers/blob/master/images/fig5.png)
<div align='center'> 
	<h4>Figure # - Replicating servers</h4>
</div>

* *C - Client*
* *SR - Server replica*

In *Figure #*, it can be noticed that a client is still provided with a single service. On the other hand, there are many server replicas that provide that particular service. They are completely transparent to a client. Due to that, there must exist a defined protocol that coordinates client interactions with server replicas.

Let us consider a following situation: client *C* connects to a server replica *SR*, and communicates with it in order to obtain responses and send requests. Throughout time, that *SR* becomes faulty (e.g. crashes and is no longer available). The aforementioned protocol must re-connect a client to another server replica, which will then continue to provide the client with responses, with all that being completely transparent to the client. Question arises - *"How will the new server replica know what the last one was doing?"*. Additionally, all *SR* can potentially provide the service to different clients. 

The solution that the ***state machine*** approach defines is - every *SR* will be implemented as a deterministic state machine, which consists of states and transitions, and whenever a client sends a request to be executed, it will be executed on all *SR* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzMzMjI1ODkyLC00Mzk4MTE3MTAsLTgyND
gxMDgwMCwxMDQ3MTU5NzU2LC0xNDE3MzkyNzk5LDE0NzU4ODI2
NTEsMTc0ODE3OTY4XX0=
-->