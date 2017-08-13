HFile represents the real data storage fle. The fles contain a variable number of data

blocks and a fxed number of fle info blocks and trailer blocks. The index blocks

records the offsets of the data and meta blocks. Each data block contains a magic

header and a number of serialized KeyValue instances.

![](/images/HFile.png)

The default size of the block is 64 KB and can be as large as the block size. Hence,

the default block size for fles in HDFS is 64 MB, which is 1,024 times the HFile

default block size but there is no correlation between these two blocks. Each

key-value in the HFile is represented as a low-level byte array.

![](/images/HFileBlock.png)

Data is stored in an HFile which contains sorted key/values. When the MemStore accumulates enough data, the entire sorted KeyValue set is written to a new HFile in HDFS. This is a sequential write. It is very fast, as it avoids moving the disk drive head.





An HFile contains a multi-layered index which allows HBase to seek to the data without having to read the whole file. The multi-level index is like a b+tree:

* Key value pairs are stored in increasing order
* Indexes point by row key to the key value data in 64KB “blocks”
* Each block has its own leaf-index
* The last key of each block is put in the intermediate index
* The root index points to the intermediate index

The trailer points to the meta blocks, and is written at the end of persisting the data to the file. The trailer also has information like bloom filters and time range info. Bloom filters help to skip files that do not contain a certain row key. The time range info is useful for skipping the file if it is not in the time range the read is looking for.

![](/assets/HFile.png)

