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


<!--stackedit_data:
eyJoaXN0b3J5IjpbNzU4MDkxMzMwLC04MjQ4MTA4MDAsMTA0Nz
E1OTc1NiwtMTQxNzM5Mjc5OSwxNDc1ODgyNjUxLDE3NDgxNzk2
OF19
-->