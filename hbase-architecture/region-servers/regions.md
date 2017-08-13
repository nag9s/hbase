HBase Tables are divided horizontally by row key range into “Regions.”

* A region contains all rows in the table between the region’s start key and end key. 
* Regions are assigned to the nodes in the cluster, called “Region Servers,” and these serve data for reads and writes. 
* A region server can serve about 1,000 regions.
*  Each region is 1GB in size \(default\)

![](/images/Region.png)

The basic unit of scalability and load balancing in HBase is called a region. These are essentially contiguous ranges of rows stored together. They are dynamically split by the system when they become too large. Alternatively, they may also be merged to reduce their number and required storage files. An HBase system ma y have more than one region servers.

* Initially there is only one region for a table and as we start adding data to it, the system is monitoring to ensure that you do not exceed a configured maximum size. If you exceed the limit, the region is split into two at the middle key middle of the region, creating two roughly equal halves.
* Each region is served by exactly one region server, the row key in the and each of these servers can serve many regions at any time.
* Rows are grouped in regions and may be served by different servers



