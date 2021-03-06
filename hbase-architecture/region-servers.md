Regions are further assigned to RegionServers \(also known as HBase cluster slaves\), hence Region Servers are responsible for all read and write requests for all regions they serve and also split regions that have exceeded the configured region size thresholds. The region servers can be added or removed while the system is u p and running to accommodate changing workloads.

**HRegionServer**

is the RegionServer implementation. It is responsible for serving and managing regions. In a distributed cluster, a RegionServer runs on a DataNode. Each Region Server is responsible to serve a set of regions, and one Region \(i.e. range of rows\) can be served only by one Region Server.

RegionServers encapsulate the storage machinery in HBase. As you saw in the architectural diagram, they’re collocated with the HDFS DataNode for data locality.

Every RegionServer **has two components shared across all contained Regions: the HLog and the BlockCache.** [**HLog**](/hbase-architecture/region-servers/hlog.md)**, also called the Write-ahead log, or WAL, is what provides HBase with data durability in the face of failure.** Every write to HBase is recorded in the HLog, written to HDFS.

The **BlockCache is the portion of memory where HBase caches data read off of disk between reads.** It’s also a major source of operational headache for HBase when configured to be too large. If you hear about HBase GC configuration woes, they stem largely from this component. There is a single block cache per region server.

![](/images/regionserver.png)

RegionServers host multiple Regions.

* A Region consists of **multiple “**[**Stores.**](/hbase-architecture/region-servers/storehstore-or-memstore.md)**” \(Hstore\) \(MemStore\)**
* **Each Store corresponds to a column family from the logical model.** Remember that business of HBase being a column family oriented database? These Stores provide that physical isolation. 
* A Store consists of multiple StoreFiles plus a MemStore. Data resident on disk is managed by the StoreFiles and is maintained in the HFile format. The MemStore accumulates edits and once filled is flushed to disk, creating new HFiles.

![](/images/region_server_inernal.png)

A Region Server runs on an HDFS data node and has the following components:

* WAL: Write Ahead Log is a file on the distributed file system. The WAL is used to store new data that hasn't yet been persisted to permanent storage; it is used for recovery in the case of failure.
* BlockCache: is the read cache. It stores frequently read data in  memory. Least Recently Used data is evicted when full.
* MemStore: is the write cache. It stores new data which has not yet been written to disk. It is sorted before writing to disk. There is one MemStore per column family per region.
* Hfiles store the rows as sorted KeyValues on disk.

![](/assets/import.png)

RegionServers are typically collocated with HDFS DataNodes  on the same physical hardware,** although that’s not a requirement.** The only requirement is that RegionServers should be able to access HDFS. They’re essentially clients and store/access data on HDFS. The master process does the distribution of regions among RegionServers, and each RegionServer typically hosts multiple regions.

![](/assets/RegionServersDataNodes.png)

Given that the underlying data is stored in HDFS, which is available to all clients as  
 a single namespace, all RegionServers have access to the same persisted files in the file  
 system and can therefore host any region . By physically collocating DataNodes and RegionServers, you can use the **data locality property**; that is, RegionServers can theoretically read and write to the local DataNode as the primary DataNode.

![](/assets/RegionSDataNodes.png)

#### When a region is assigned to a RegionServer, how does my client application \(the one doing reads and writes\) know its location?

Two special tables in HBase, -ROOT-\( this is deprecated since hbase 0.96, but the functionality remains same\) and .META., help find where regions for various tables are hosted. Like all tables in HBase, -ROOT- and .META. are also split into regions. -ROOT- and .META. are both special tables, but -ROOT- is more special than .META.; -ROOT- never splits into more than one region. .META. behaves like all other tables and can split into as many regions as required.

