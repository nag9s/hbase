whether the special tables - ROOT and META will be replicated ? If not , if ROOT is down , how it will be handled ?

How ROOT . META will be populated for the first time ? How they are involved in the WRITE path ?  - check Hbase Premier 10th chapter

Book - Learning Hbase

**chapter 2  Reading  ,writing cycle**

Each column family might have many HFiles, but the HFile will only belong to a specific column family.  does this mean entire column family or the only few columns those got updates ?

Region assignment  - check once , it is not clear   - it was referring to a case if the region assignment is correct and valid  - why and what are the cases region assignment is incorrect and not  valid  - take a look at regionserver failover section

