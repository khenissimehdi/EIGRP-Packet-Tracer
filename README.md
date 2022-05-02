# EIGRP-Packet-Tracer

# 1. Introduction

Enhanced Interior Gateway Routing Protocol (EIGRP) is an advanced distance-vector routing protocol that is used on a computer network for automating routing decisions and configuration. The protocol was designed by Cisco Systems as a proprietary protocol, available only on Cisco routers. 
EIGRP is used on a router to share routes with other routers within the same autonomous system. Unlike other well known routing protocols, such as RIP, EIGRP only sends incremental updates, reducing the workload on the router and the amount of data that needs to be transmitted.
EIGRP replaced the Interior Gateway Routing Protocol (IGRP) in 1993. One of the major reasons for this was the change to classless IPv4 addresses in the Internet Protocol, which IGRP could not support.

EIGRP is a dynamic routing protocol by which routers automatically share route information. This eases the workload on a network administrator who does not have to configure changes to the routing table manually.
In addition to the routing table, EIGRP also stores the neighbour table and the topology table.



# Configuring EIGRP on the routers

First, to enter EIGRP mode we have to choose an Autonomous System  (number) and enter the following:

```
router eigrp 90
no auto-summary
```

Then we have to specify which network should be integrated in the EIGRP config
We can either use a wildcardmask or use the exact ip.

``` 
network 172.30.0.0 0.0.255.255
```

So, on our EIGRP routers we have to include neighbours networks ot include as such : 
R1 & R2 
```
router eigrp 90
 network 10.1.0.0 0.0.255.255
 network 192.168.0.0 0.0.0.3
 network 192.168.0.8 0.0.0.3
```

# R3 
in this router we are going do basically the same thing as the other routers but this time we are going to add the ability to redistribute the default route that lead the hots to the internet
because he is the only one that can see it.
```
router eigrp 90
redistribute static  -> allow the router to redisstribute the static route
network 192.168.0.0 0.0.0.3 
network 192.168.0.4 0.0.0.3
network 0.0.0.0 -> the static route 
ip route 0.0.0.0 255.255.255.255 GigabitEthernet0/0
```



# NAT Configuration
NAT enables private IP internet works that use Unregistered IP addresses to connect to the Internet.
NAT operates on a device, usually connecting two networks. Before packets are forwarded onto another network, 
NAT translates the private (not globally unique) addresses in the internal network into legal addresses.
NAT can be configured to advertise to the outside world only one address for the entire network. 
This ability provides more security by effectively hiding the entire internal network behind that one address.
-- FROM CISCO

In Our Case to Configure this first you will have to pick your outside in inside interface go the router
and do the following :

1. Inside :
```
interface <Interface-Name>
ip nat inside
```
2. Outside
```
interface <Interface-Name>
ip nat outside
```
Now we are going to use a method that will allow the PAT(Port Address Translation) this will help the machines connect
to the internet using a single public ip address but use the port to translate the response back to the machine that 
requested it 

```
ip nat inside source list 1 interface <OUT-INTERFACE> overload
```

# Adding default route 
When the destination is not known the machine will go the catch-all address that is 0.0.0.0 the problem that this 
address lead no where, so we will have to do that when the machines don't find the address and the packet is sent to the
0.0.0.0 the packet will translate to another ip that we're going to pick in our case 200.10.1.18 and, we are going to 
pick also an interface that will hold the 0.0.0.0

```
ip route 0.0.0.0 0.0.0.0 200.10.1.18 
ip route 0.0.0.0 255.255.255.255 GigabitEthernet0/0 
```

# Adding ACLs
Here we will need to add the acl so that the router can send back the information to router 3 and that acl just permit
the network 10.0.0.0

```
access-list 1 permit 10.0.0.0 0.255.255.255
```

