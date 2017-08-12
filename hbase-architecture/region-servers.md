 Regions are further assigned to RegionServers \(also known as HBase cluster slaves\), hence Region Servers are responsible for all read and write requests for all regions they serve and also split regions that have exceeded the configured region size thresholds. The region servers can be added or removed while the system is u p and running to accommodate changing workloads. 



**HRegionServer**

 is the RegionServer implementation. It is responsible for serving and managing regions. In a distributed cluster, a RegionServer runs on a DataNode. Each Region Server is responsible to serve a set of regions, and one Region \(i.e. range of rows\) can be served only by one Region Server.







![](/images/regionserver.png)

