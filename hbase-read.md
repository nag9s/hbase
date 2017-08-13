There is a special HBase Catalog table called the META table, which holds the location of the regions in the cluster. ZooKeeper stores the location of the META table.

This is what happens the first time a client reads or writes to HBase:

1. The client gets the Region server that hosts the META table from ZooKeeper.
2. The client will query the .META. server to get the region server corresponding to the row key it wants to access. The client caches this information along with the META table location.
3. It will get the Row from the corresponding Region Server.

For future reads, the client uses the cache to retrieve the META location and previously read row keys. Over time, it does not need to query the META table, unless there is a miss because a region has moved; then it will re-query and update the cache.

![](/assets/HbaseRead.png)

* As a general rule, if we need fast access to data, we should keep it ordered and as much of it as possible in memory.

* HBase accomplishes both of these goals, allowing it to serve millisecond reads in most cases. 
  A read against HBase must be reconciled between the persisted HFiles and the data still in the MemStore. HBase has an LRU cache for reads. This cache, also called BlockCache , remains in the JVM heap along with MemStore.
* The BlockCache is designed to keep frequently accessed data from the HFiles in memory so as to avoid disk reads as much as possible. Each column family has its own BlockCache.
   Understanding the BlockCache is an important part of understanding how to run HBase at optimal performance. 
  The "Block" in BlockCache is the unit of data that HBase reads from disk in a single pass.
   The HFile is physically laid out as a sequence of blocks plus an index over those blocks, this means reading a block from HBase requires only looking up that block's location in the index and retrieving it from disk.
* The block is the smallest indexed unit of data and is the smallest unit of data that can be read from disk. The block size is configured per column family, and the default value is 64 KB. We may tweak this value as per our usecase.
   For random lookups a smaller block size will be recommended but smaller blocks creates a larger index and thereby consumes more memory. For more sequential scans, reading many blocks at a time, we should have larger block size. This allows us to save on memory because larger blocking this way we will have fewer index entries and thus a smaller index.
* Reading a row from HBase requires first checking the MemStore for any pending modifications. Then the BlockCache is examined to see if the block containing this row has been recently accessed. Finally, the relevant HFiles on disk are accessed. There are more things going on under the hood, but this is the overall outline.
* Note that HFiles contain a snapshot of the MemStore at the point when it was flushed. Data for a complete row can be stored across multiple HFiles. In order to read a complete row, HBase must read across all HFiles that might contain information for that row in order to compose the complete record. 



