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

Let us consider a following situation: client *

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwODI0OTI4NzcsLTQzOTgxMTcxMCwtOD
I0ODEwODAwLDEwNDcxNTk3NTYsLTE0MTczOTI3OTksMTQ3NTg4
MjY1MSwxNzQ4MTc5NjhdfQ==
-->