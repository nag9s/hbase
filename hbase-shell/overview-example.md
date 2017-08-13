CoreServlets - Hbase Installation



Let's walk through an example

1. Create a table

• Define column families

2. Populate table with data records

• Multiple records

3. Access data

• Count, get and scan

4. Edit data

5. Delete records

6. Drop table





1. Create a table



Create table called 'Blog' with the following schema

– 2 families

• 'info' with 3 columns: 'title', 'author', and 'date'

• 'content' with 1 column family: 'post'￼	





Various options to create tables and

families

– hbase&gt; create 't1', {NAME =&gt; 'f1', VERSIONS =&gt; 5}

– hbase&gt; create 't1', {NAME =&gt; 'f1', VERSIONS =&gt; 1,

TTL =&gt; 2592000, BLOCKCACHE =&gt; true}

– hbase&gt; create 't1', {NAME =&gt; 'f1'}, {NAME =&gt; 'f2'},

{NAME =&gt; 'f3'}

– hbase&gt; create 't1', 'f1', 'f2', 'f3'







hbase&gt; create 'Blog', {NAME=&gt;'info'}, {NAME=&gt;'content'}

0 row\(s\) in 1.3580 seconds.

Note , column families need to be defined at the table creation time.

So, the above will create a table with Blog having families info, content.



NAME is keyword in hbase that will take .





Assignment operator will be  =&gt; in jRuby \( Hbase shll will work in jRuby\). So the statment 



NAME=&gt;'info' , will assign info to the one of the column familes









2: Populate Table With Data

Records







Put command format:

hbase&gt; put 'table', 'row\_id', 'family:column', 'value'







put 'Blog', 'Matt-001', 'info:title', 'Elephant'

put 'Blog', 'Matt-001', 'info:author', 'Matt'

put 'Blog', 'Matt-001', 'info:date', '2009.05.06'

put 'Blog', 'Matt-001', 'content:post', 'Do elephants like monkeys?'











Please note



1. colums need not be defined create table time. thus , put can be used to add columns along it s values



2. coumn is qualified using 

columqualifier :  coulmnname \(info:date\)













