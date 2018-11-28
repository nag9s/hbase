**JPS is not showing HregionServer**

it might happen - `hbase.cluster.distributed `is not set or set to false in `hbase-site.xml`



hbase.cluster.distributed :  
The mode the cluster will be in. Possible values are false for standalone mode and true for distributed mode. If false, startup will run all HBase and ZooKeeper daemons together in the one JVM. Default: false

So if you set it to**true**you'll see the distinct master, region server and ZooKeeper processes.



&lt;property&gt;

    &lt;name&gt;hbase.cluster.distributed&lt;/name&gt;

    &lt;value&gt;true&lt;/value&gt;

  &lt;/property&gt;

