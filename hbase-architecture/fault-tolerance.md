### HMaster Failures

One may think that the Master is a SPOF \(single point of failure\). Actually, we can set up multiple HMasters although only one is active .

* HMasters use heartbeats to monitor each other.
* If the active Master shuts down or loses its lease in ZooKeeper, the remaining Masters jostle to take over the Master role.
* Because the clients talk directly to the RegionServers, the HBase cluster can still function in a steady state in short period during the Master failover. Note that Accumulo doesn’t support multiple Masters currently and thus the Master is a SPOF.

### RegionServers Failures

**So how about RegionServers**? It looks like that we are safe since there are multiple instances. **However, recall that a region is managed by a single RegionServer at a time. If a RegionServer fails, the corresponding regions are not available until the detection and recovery steps have happened. ** Zookeeper will determine Node failure when it loses region server heart beats. The HMaster will then be notified that the Region Server has failed. **It is actually a SPOF although there are no global failures in HBase**.

When the HMaster detects that a region server has crashed, the HMaster reassigns the regions from the crashed server to active Region servers. In order to recover the crashed region server’s memstore edits that were not flushed to disk. The HMaster splits the WAL belonging to the crashed region server into separate files and stores these file in the new region servers’ data nodes. Each Region Server then replays the WAL from the respective split WAL, to rebuild the memstore for that region.

To be resilient to node failures, all StoreFiles are written into HDFS, which replicates the blocks of these files \(3 times by default\). Besides, HBase, just like any other durable databases, uses a write-ahead-log \(WAL\), which is also written into HDFS. To detect the silent death of RegionServers, HBase uses ZooKeeper. Each RegionServer is connected to ZooKeeper and the Master watches these connections. ZooKeeper itself employs heartbeats. On a timeout, the Master declares the RegionServer as dead and starts the recovery process. **During the recovery, the regions are reassigned to random RegionServers and each RegionServer reads the WAL to recover the correct region state. This is a complicated process and the mean time to recovery \(MTTR\) of HBase is often around 10 minutes if a DataNode crash with default settings. But we may reduce the MTTR to less than 2 minutes with **[careful settings.](http://hortonworks.com/blog/introduction-to-hbase-mean-time-to-recover-mttr/)

![](/assets/RegionServerFailure.png)

### **DataNode Failover**

* These are handled by HDFS replication \(out of the box as part of Hadoop deployment\)

### Data Recovery

WAL files contain a list of edits, with one edit representing a single put or delete. Edits are written chronologically, so, for persistence, additions are appended to the end of the WAL file that is stored on disk.

What happens if there is a failure when the data is still in memory and not persisted to an HFile? The WAL is replayed. Replaying a WAL is done by reading the WAL, adding and sorting the contained edits to the current MemStore. At the end, the MemStore is flush to write changes to an HFile.

![](/assets/dataRecovery.png)

