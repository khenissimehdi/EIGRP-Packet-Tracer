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
