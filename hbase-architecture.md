# Hbase Architecture

HBase is composed of three types of servers in a master slave type of architecture.

* **Region servers** serve data for reads and writes. When accessing data, clients communicate with HBase RegionServers directly. 
* **HBase Master**  Region assignment, DDL \(create, delete tables\) operations are handled by the HBase Master process. 
* **Zookeeper**, which is part of HDFS, maintains a live cluster state.



