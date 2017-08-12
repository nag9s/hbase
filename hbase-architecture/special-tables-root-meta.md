Hbase In Action

* Two special tables in HBase, -ROOT- and .META., help find where regions for various tables are hosted. 
* Like all tables in HBase, -ROOT- and .META. are also split into   regions. -ROOT- and .META. are both special tables, but -ROOT- is more special than   .META.; -ROOT- never splits into more than one region. 
* .META. behaves like all other   tables and can split into as many regions as required.



