* A block cache instance is created at the time of the region server startup and it can have   an implementation of LruBlockCache, SlabCache, or BucketCache. 
* The block cache   also supports multilevel caching; that is, a block cache might have frst-level cache,   L1, as LruBlockCache and second-level cache, L2, as SlabCache or BucketCache.
* All these cache implementations have their own way of managing the memory; for   example, LruBlockCache is like a data structure and resides on the JVM heap whereas   the other two types of implementation also use memory outside of the JVM heap.



