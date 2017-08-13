When the client issues a Put request, the first step is to write the data to the write-ahead log, the WAL:

* Edits are appended to the end of the WAL file that is stored on disk.

* The WAL is used to recover not-yet-persisted data in case a server crashes.

![](/assets/HBase Write Steps %281%29.png)

Once the data is written to the WAL, it is placed in the MemStore. Then, the put request acknowledgement returns to the client.

![](/assets/HBase Write Steps %282%29.png)



 Whether we use add a new row in HBase or to modify an existing row, the internal process remains the same . HBase recieve the call and and persist the change. When a write is made, by default, it goes to two places:

* **MemStore**
* * MemStore is a write buffer\(64MB by default\). When the data in MemStore accumulates its threshold, data will be flush to a new HFile on HDFS persistently. Each Column Family can have many HFiles, but each HFile only belongs to one Column Family.
* **Write-ahead log WAL \(also referred to as the HLog\) **
* * WAL is for data reliability, WAL is persistent on HDFS and each Region Server has only on WAL. When the Region Server is down before MemStore flush, HBase can replay WAL to restore data on a new Region Server.

  


 The default behavior of HBase is to write in both places to maintain data durability. Only after the change is written to and confirmed in both the places is the write considered complete. The MemStore is a write buffer where HBase accumulates data in memory before a permanent write. Its contents are flushed to disk to form an HFile when the MemStore fills up. It does not write to an existing HFile instead it forms a new file on every flush. The HFile is the underlying storage format for HBase. HFiles belong to a column family and and a column family can have multiple HFiles. But a single HFile can't have data for multiple column families. There is one MemStore per column family.

  


  


 Failures are common in large distributed systems, and HBase is no exception. Imagine that the server hosting a MemStore that has not been yet flushed crashes. We'll lose the data that was in memory but not yet persisted. HBase safeguards against that by writing to the WAL before the write completes. Every server that's part of the HBase cluster keeps a WAL to record changes as they happen. The WAL is a file on the underlying file system. A write isn't considered successful until the new WAL entry is successfully written. This guarantee makes HBase as durable as the file backing it. Most of the time, HBase is backed by the Hadoop Distributed Filesystem \(HDFS\). If HBase goes down, the data that not yet flushed from the MemStore to the HFile can be recovered by replaying the WAL. We don't have to do this manually. It's all handled under the hood by HBase as a part of the recovery process. There is a single WAL per HBase server shared by all tables \(and their column families\) served from that server.

  


  


 As we can imagine, skipping the WAL during writes can help improve write performance. There's one less thing to do, right? We don't recommend disabling the WAL unless we're willing to lose data when things fail. In case we want to experiment, we can disable the WAL like this \(client APIs will be discussed later\):

  


  


**Put p = new Put\(\);**

**  
**

** p.setWriteToWAL\(false\);**

**  
**

  


 Note that every write to HBase requires confirmation from both the WAL \(Write Ahead Log\) and the MemStore. The two steps ensure that every write to HBase happens as fast as possible while maintaining durability. The MemStore is flushed to a new HFile when it fills up.

![](/assets/Hbase Write Path.png)

