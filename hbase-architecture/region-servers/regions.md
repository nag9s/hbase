HBase Tables are divided horizontally by row key range into “Regions.”

*  A region contains all rows in the table between the region’s start key and end key. 
* Regions are assigned to the nodes in the cluster, called “Region Servers,” and these serve data for reads and writes. 
* A region server can serve about 1,000 regions.

![](/images/Region.png)







