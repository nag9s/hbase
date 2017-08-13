## Read Load Balancing {#read-load-balancing}

Splitting happens initially on the same region server, but for load balancing reasons, the HMaster may schedule for new regions to be moved off to other servers. This results in the new Region server serving data from a remote HDFS node until a major compaction moves the data files to the Regions server’s local node. HBase data is local when it is written, but when a region is moved \(for load balancing or recovery\), it is not local until major compaction.



![](/assets/readloadBal.png)

