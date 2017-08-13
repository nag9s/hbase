

**CoreServlets  Hbase Installation and Shellf**



Quote all names   

– Table and column names

– Single quotes for text

• hbase&gt; get 't1', 'myRowId'

– Double quotes for binary

• Use hexadecimal representation of that binary value

• hbase&gt; get 't1', "key\x03\x3f\xcd"





Uses ruby hashes to specify parameters

– {'key1' =&gt; 'value1', 'key2' =&gt; 'value2', …}

– Example:







**Hbase Definitive Guide**



**Quote names**



Commands that require a table or column name expect the name to be quoted in

either single or double quotes.





Quote values



The shell supports the output and input of binary values using a hexadecimal—or

octal—representation. You must use double quotes or the shell will interpret them

as literals.



hbase&gt; get 't1', "key\x00\x6c\x65\x6f\x6e"

hbase&gt; get 't1', "key\000\154\141\165\162\141"

hbase&gt; put 't1', "test\xef\xff", 'f1:', "\x01\x33\x70"



Note the mixture of quotes: you need to make sure you use the correct ones, or

the result might not be what you had expected. Text in single quotes is treated as

a literal, whereas double-quoted text is interpolated, that is, it transforms the octal,

or hexadecimal, values into bytes.





Comma delimiters for parameters



Separate command parameters using commas. For example:

hbase\(main\):001:0&gt; get 'testtable', 'row-1',

'colfam1:qual1'



Ruby hashes for properties



For some commands, you need to hand in a map with key/value properties. This

is done using Ruby hashes:

{'key1' =&gt; 'value1', 'key2' =&gt; 'value2', ...}

The keys/values are wrapped in curly braces, and in turn are separated by "=&gt;".

Usually keys are predefined constants such as NAME, VERSIONS, or COMPRESSION, and

do not need to be quoted. For example:

hbase\(main\):001:0&gt; create 'testtable', {NAME =&gt;

'colfam1', VERSIONS =&gt; 1, \

TTL =&gt; 2592000, BLOCKCACHE =&gt; true}





For any command, you can get detailed help by typing in help '&lt;command&gt;'. Here’s an

example:

hbase\(main\):001:0&gt; help 'status'

Show cluster status. Can be 'summary', 'simple', or 'detailed'. The

default is 'summary'. Examples:

hbase&gt; status

hbase&gt; status 'simple'

hbase&gt; status 'summary'

hbase&gt; status 'detailed'

The majority of commands have a direct match with a method provided by either the

client or administrative API. Next is a brief overview of each command and the matching API functionality.

