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

