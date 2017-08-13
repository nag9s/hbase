* In the case of writing the data, when the client calls HTable.put\(Put\), the data is  frst written to the write-ahead log fle \(which contains actual data and sequence  numbers together represented by the HLogKey class\) and also written in MemStore.
* Writing data directly into MemStrore can be dangerous as it is a volatile in-memory  buffer and always open to the risk of losing data in case of a server failure. 
* Once  MemStore is full, the contents of the MemStore are ﬂushed to the disk by creating  a new HFile on the HDFS.

* If there is a server failure, the WAL can effectively retrieve the log to get everything   up to where the server was prior to the crash failure. Hence, the WAL guarantees   that the data is never lost. 
* Also, as another level of assurance, the actual write-ahead   log resides on the HDFS, which is a replicated flesystem. Any other server having a   replicated copy can open the log.

* The HLog class represents the WAL. When an HRegion object is instantiated, the   single HLog instance is passed on as a parameter to the constructor of HRegion. In   the case of an update operation, it saves the data directly to the shared WAL and
* also keeps track of the changes by incrementing the sequence numbers for each edit.
* WAL uses a Hadoop SequenceFile, which stores records as sets of key-value pairs.  Here, the HLogKey instance represents the key, and the key-value represents the   rowkey, column family, column qualifer, timestamp, type, and value along with the   region and table name where data needs to be stored. 
* Also, the structure starts with   two fxed-length numbers that indicate the size and value of the key.

The following diagram shows the structure of a key-value pair:



![](/images/WAL.png)

