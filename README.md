# Assignment-3.5
q1. Explain high availability of namenode?
Ans: The HDFS NameNode High Availability feature enables you to run redundant NameNodes in the same cluster in an Active/Passive configuration with a hot standby. This eliminates the NameNode as a potential single point of failure (SPOF) in an HDFS cluster.
Formerly, if a cluster had a single NameNode, and that machine or process became unavailable, the entire cluster would be unavailable until the NameNode was either restarted or started on a separate machine. This situation impacted the total availability of the HDFS cluster in two major ways:

1.In the case of an unplanned event such as a machine crash, the cluster would be unavailable until an operator restarted the NameNode.
2.Planned maintenance events such as software or hardware upgrades on the NameNode machine would result in periods of cluster downtime.

q2. What is check pointing and how it is useful?
Ans : Chckpointing is an essential part of maintaining and persisting filesystem metadata in HDFS.At a high level, the NameNode’s primary responsibility is storing the HDFS namespace. This means things like the directory tree, file permissions, and the mapping of files to block IDs. It’s important that this metadata (and all changes to it) are safely persisted to stable storage for fault tolerance.
This filesystem metadata is stored in two different constructs: the fsimage and the edit log. 
The fsimage is a file that represents a point-in-time snapshot of the filesystem’s metadata. However, while the fsimage file format is very efficient to read, it’s unsuitable for making small incremental updates like renaming a single file. Thus, rather than writing a new fsimage every time the namespace is modified, the NameNode instead records the modifying operation in the edit log for durability. This way, if the NameNode crashes, it can restore its state by first loading the fsimage then replaying all the operations (also called edits or transactions) in the edit log to catch up to the most recent state of the namesystem. The edit log comprises a series of files, called edit log segments, that together represent all the namesystem modifications made since the creation of the fsimage.

q3.What is HDFS Federation?
Ans : HDFS Federation improves the existing HDFS architecture through a clear separation of namespace and storage, enabling generic block storage layer. It enables support for multiple namespaces in the cluster to improve scalability and isolation. Federation also opens up the architecture, expanding the applicability of HDFS cluster to new implementations and use cases.

HDFS has two main layers:
1.Namespace manages directories, files and blocks. It supports file system operations such as creation, modification, deletion and listing of files and directories.
2.Block Storage has two parts:
1)Block Management maintains the membership of datanodes in the cluster. It supports block-related operations such as creation, deletion, modification and getting location of the blocks. It also takes care of replica placement and replication.
2)Physical Storage stores the blocks and provides read/write access to it.
