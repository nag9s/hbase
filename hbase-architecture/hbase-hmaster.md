Region assignment, DDL \(create, delete tables\) operations are handled by the HBase Master.

A master is responsible for:

* Coordinating the region servers

  - Assigning regions on startup , re-assigning regions for recovery or load balancing
  - Monitoring all RegionServer instances in the cluster \(listens for notifications from zookeeper\)

* Admin functions

  - Interface for creating, deleting, updating tables



![](/images/HBase Hmaster.png)



