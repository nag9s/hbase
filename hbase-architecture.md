# Hbase Architecture

HBase is composed of three types of servers in a master slave type of architecture.

* **Region servers** serve data for reads and writes. When accessing data, clients communicate with HBase RegionServers directly. 
* **HBase Master**  Region assignment, DDL \(create, delete tables\) operations are handled by the HBase Master process. 
* **Zookeeper**, which is part of HDFS, maintains a live cluster state.

![](/assets/HbaseArch1.png)

**One Master Server **

* * The master is responsible for assigning regions to region servers and uses Apache ZooKeeper , a reliable, highly-available, persistent, and distributed coordination service, to facilitate that task. 
  * The master server is also responsible for handling load balancing of regions across region servers, to unload busy servers and move regions to less occupied ones. In addition, it takes care of schema changes and other metadata operations, such as creation of tables and column families.
  * Technically 
    **HMaster**
     is the implementation of the Master Server. The Master server is responsible for monitoring all RegionServer instances in the cluster, and is the interface for all metadata changes. In a distributed cluster, the Master typically runs on the NameNode. A cluster may have multiple masters, all Masters compete to run the cluster. If the active Master loses its lease in ZooKeeper \(or the Master shuts down\),  then the remaining Masters jostle to take over the Master role.   
* **Multiple Region Servers**
* * In HBase tables are partitioned into Regions. Region defined by start 
    &
     end row keys, these are basically smallest unit of distribution. Regions are further assigned to RegionServers \(also known as HBase cluster slaves\), hence Region Servers are responsible for all read and write requests for all regions they serve and also split regions that have exceeded the configured region size thresholds. The region servers can be added or removed while the system is u p and running to accommodate changing workloads. 
  *  Technically 
    **HRegionServer**
     is the RegionServer implementation. It is responsible for serving and managing regions. In a distributed cluster, a RegionServer runs on a DataNode. Each Region Server is responsible to serve a set of regions, and one Region \(i.e. range of rows\) can be served only by one Region Server.
* **HBase Clients**
* * End clients, can be commandline, Java, thrift or REST based. 

![](/assets/HbaseArch.png)

# Hbase Table Objects![](/assets/Hbase Table Objects.png)



