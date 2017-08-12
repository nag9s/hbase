 Like a normal SQL/NoSQL stores we have similar concepts of Tables, rows, columns, cells here:

  


* The most basic unit is a column.
* One or more columns form a row that is addressed uniquely by a row key.
* A number of rows, in turn, form a table, and there can be many of them hence we it looks similar to that to RDBMS table i.e. rows are comprised of columns, and those are in turn are grouped into column families
* Each column may have multiple versions, with each distinct value contained in a separate cell.
* All rows are always sorted lexicographically by their row key.
* All column members of a column family have a common prefix. For example, the columns 
  **courses:history**
   and 
  **courses:math**
   are both members of the courses column family. Physically, all column family members are stored together on the filesystem in the same low-level storage files, called  **HFile**
  . Because tunings and storage specifications are done at the column family level, it is advised that all column family members have the same general access pattern and size characteristics. 
* It can be expensive to add new column families so, column families need to be defined when the table is created and should not be changed too often, nor should there be too many. Fortunately, a column family may have any number of columns. Each column family may have its own rules regarding how many versions of a given cell to keep. All the columns within a column family will share the same characteristics such as versioning and compression \(The name of the column family must be printable characters, a notable difference to all other names or values.
* Columns are often referenced as family:qualifier with the qualifier being any arbitrary array of bytes. You could have millions of columns in a particular column family. There is also no type nor length boundary on the column values.
* Every column value, or cell, is either timestamped implicitly by the system or can be set explicitly by the user. This can be used, to save multiple versions of a value as it changes over time. Different versions of a cell are stored in decreasing timestamp order, allowing you to read the newest value first.
* The user can specify how many versions of a value should be kept. In addition, there is support for predicate deletions allowing you to keep, for example, only values written in the last week.
* The BigTable model, as implemented by HBase, is a sparse, distributed, persistent, multi dimensional map, which is indexed by row key, column key, and a timestamp. Putting this together, we can express the access to data like so:
 
 
  **\(Table, RowKey, Family, Column, Timestamp\) =**
  **&gt;**
  ** Value**
 
 
   In a more programming language style like in Java, we can represent it as \(just to visualize and understand\)
 
 
  **SortedMap**
  **&lt;**
  **RowKey, List**
  **&lt;**
  **SortedMap**
  **&lt;**
  **Column, List**
  **&lt;**
  **Value, Timestamp**
  **&gt;**
  **&gt;**
  **&gt;**
  **&gt;**
 
 
  The first SortedMap is the table, containing a List of column families. The families contain another SortedMap, which represent the column and their associated values. These values are in the final List, that holds the value and the timestamp.



