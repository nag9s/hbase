Hbase In Action

* Two special tables in HBase, -ROOT- and .META., help find where regions for various tables are hosted. 
* Like all tables in HBase, -ROOT- and .META. are also split into
   regions. -ROOT- and .META. are both special tables, but -ROOT- is more special than
   .META.; -ROOT- never splits into more than one region. 
* .META. behaves like all other
   tables and can split into as many regions as required.

## HBase Meta Table {#hbase-meta-table}

* This META table is an HBase table that keeps a list of all regions in the system.
* The .META. table is like a b tree.
* The .META. table structure is as follows:

  - Key: region start key,region id
  - Values: RegionServer



