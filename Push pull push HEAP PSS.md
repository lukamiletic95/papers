### Push-pull-push HEAP with PSS

The idea of this algorithm combines a three-phase gossiping protocol described in [11], a protocol aware of network bandwidth inequality - _**HEAP**_ (**HE**terogeneity-**A**ware-**P**rotocol)  [12], and a gossiping service for providing every node with its peer subset - the _**PSS**_ (**P**eer **S**ampling **S**ervice) [13]. Therefore, this algorithm consists of three parts, which are slightly modified in order to be able to be applied to our system model:

1. _Push-pull-push gossiping_
2. _HEAP_
3. _PSS_

Each of the three parts will be explained in further text.

<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig15.png" />
	<h4>Figure # - Messages exchanged during gossiping</h4>
</div>
<br/><br/>


<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig16.png" />
	<h4>Figure # - Additional HEAP messages</h4>
</div>
<br/><br/>


<br/><br/>
<div align='center'> 
<img src="https://github.com/lukamiletic95/papers/blob/algorithm3/images/fig17.png" />
	<h4>Figure # - Peer sampling service</h4>
</div>
<br/><br/>
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA5MjkyMzIzMiwtMTg3OTM1MjgxMiwxMD
I5NjgwMjg3LDEyOTgwOTM5NzRdfQ==
-->