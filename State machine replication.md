### State machine replication

Let us assume a standard client-server architecture. Server provides a service for a client, and in order for a client to make use of that service, it has to send a request, and wait for a response.

<br/>
<div align='center'> 
	<img src="https://github.com/lukamiletic95/papers/blob/master/images/fig4.png" />
	<h4>Figure # - Standard client-server architecture</h4>
</div>

* *C - Client*

Although this is the simplest approach in implementing a client-server service, its main disadvantage is in having only one server. In case that server becomes faulty, the whole service will be unavailable to a client. That one server may be considered a single point of failure.

Therefore, we assume another, more advanced and reliable client-server architecture, shown in *Figure #*, which is based on replicating servers using the ***state machine*** approach.

<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/master/images/fig5.png" />
	<h4>Figure # - Replicating servers</h4>
</div>

* *C - Client*
* *SR - Server replica*

In *Figure #*, it can be noticed that a client is still provided with a single service. On the other hand, there are many server replicas that provide that particular service. They are completely transparent to a client. Due to that, there must exist a defined protocol that coordinates client interactions with server replicas. [^1]

Let us consider a following situation: client *C* connects to a server replica *SR*, and communicates with it in order to obtain responses and send requests. Throughout time, that *SR* becomes faulty (e.g. crashes and is no longer available). The aforementioned protocol must re-connect a client to another server replica, which will then continue to provide the client with responses, with all that being completely transparent to the client. Question arises - *"How will the new server replica know what the last one was doing?"*. Additionally, all *SR* can potentially provide the service to different clients. 

The solution that the ***state machine*** approach defines is - every *SR* will be implemented as a deterministic state machine, which consists of states and transitions, and whenever a client sends a request to be executed, it will be executed on all *SR*, thus leading to a transition to another state. This approach also states that transitions between states on all *SR* replicas must be exactly the same. Consequently, when a situation mentioned before occurs, since the new *SR* will be in the same state as the crashed *SR*, it can easily continue where the former one left off.

<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/master/images/fig6.png" />
	<h4>Figure # - State machine within a <i>SR</i></h4>
</div>

* *ST - State*
* *i - Input*

The state machine has to be deterministic. If it were non-deterministic, it would have the property of being able to make a transition to different states in case of the same input. Therefore, it would be possible that there is a state machine (or state machines) whose current state would be inconsistent with the states of others.

As Schneider states [7], the key for implementing this state machine is:

* **Replica coordination** - all replicas receive and process the same sequence of requests. This can be further decomposed into:

	* **Agreement** - every non-faulty *SR* receives every request.
	* **Order** - every non-faulty *SR* processes the requests it receives in the same relative order.

This approach also enables resilience to both Byzantine failures and Start-Stop failures (simply by assuming that there will never be more than a certain number of faulty/malicious *SR*). It is the main idea behind blockchain, where reaching a consensus on what the next block will be is nothing more than deciding collectively what the next state of all the state machine replicas will be.

[^1]: Discussing this particular protocol is beyond the scope of this paper.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUyNzAzNTE3LC0yNzA1MDI4ODUsNjYxOD
Y0MTc5XX0=
-->