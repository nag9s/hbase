* In the case of writing the data, when the client calls HTable.put\(Put\), the data is   frst written to the write-ahead log fle \(which contains actual data and sequence   numbers together represented by the HLogKey class\) and also written in MemStore.
* Writing data directly into MemStrore can be dangerous as it is a volatile in-memory   buffer and always open to the risk of losing data in case of a server failure. 
* Once   MemStore is full, the contents of the MemStore are ﬂushed to the disk by creating   a new HFile on the HDFS.



