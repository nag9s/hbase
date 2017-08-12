Region assignment, DDL \(create, delete tables\) operations are handled by the HBase Master.  Technically  **HMaster**

 is the implementation of the Master Server. The Master server is responsible for monitoring all RegionServer instances in the cluster, and is the interface for all metadata changes. In a distributed cluster, the Master typically runs on the NameNode. A cluster may have multiple masters, all Masters compete to run the cluster. If the active Master loses its lease in ZooKeeper \(or the Master shuts down\),  then the remaining Masters jostle to take over the Master role.   

A master is responsible for:

* Coordinating the region servers

  * Assigning regions on startup , re-assigning regions for recovery or load balancing
  * Monitoring all RegionServer instances in the cluster \(listens for notifications from zookeeper\)
  *  is responsible for assigning regions to region servers and uses Apache ZooKeeper , a reliable, highly-available, persistent, and distributed coordination service, to facilitate that task. 

* Admin functions

  * Interface for creating, deleting, updating tables
  *  responsible for handling load balancing of regions across region servers, to unload busy servers and move regions to less occupied ones. In addition, it takes care of schema changes and other metadata operations, such as creation of tables and column families.

![](/images/HBase Hmaster.png)

