When the client issues a Put request, the first step is to write the data to the write-ahead log, the WAL:

* Edits are appended to the end of the WAL file that is stored on disk.

* The WAL is used to recover not-yet-persisted data in case a server crashes.

![](/assets/HBase Write Steps %281%29.png)

Once the data is written to the WAL, it is placed in the MemStore. Then, the put request acknowledgement returns to the client.



![](/assets/HBase Write Steps %282%29.png)

