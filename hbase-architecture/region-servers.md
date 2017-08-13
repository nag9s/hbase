Regions are further assigned to RegionServers \(also known as HBase cluster slaves\), hence Region Servers are responsible for all read and write requests for all regions they serve and also split regions that have exceeded the configured region size thresholds. The region servers can be added or removed while the system is u p and running to accommodate changing workloads.

**HRegionServer**

is the RegionServer implementation. It is responsible for serving and managing regions. In a distributed cluster, a RegionServer runs on a DataNode. Each Region Server is responsible to serve a set of regions, and one Region \(i.e. range of rows\) can be served only by one Region Server.

RegionServers encapsulate the storage machinery in HBase. As you saw in the architectural diagram, they’re collocated with the HDFS DataNode for data locality.  

Every RegionServer **has two components shared across all contained Regions: the HLog and the BlockCache.** [HLog](/hbase-architecture/region-servers/hlog.md), also called the Write-ahead log, or WAL, is what provides HBase with data durability in the face of failure. Every write to HBase is recorded in the HLog, written to HDFS.

The BlockCache is the portion of memory where HBase caches data read off of disk between reads. It’s also a major source of operational headache for HBase when configured to be too large. If you hear about HBase GC configuration woes, they stem largely from this component.

![](/images/regionserver.png)

