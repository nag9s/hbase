HBase uses ZooKeeper as a distributed coordination service to maintain server state in the cluster.

Zookeeper maintains which servers are alive and available, and provides server failure notification.

Zookeeper uses consensus to guarantee common shared state. Note that there should be three or five machines for consensus.



![](/images/zookeeper.png)

