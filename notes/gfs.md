# The Google File System

S. Ghemawat, H. Gobioff, and S-T. Leung, SOSP 2003

This paper presents the Google File System (GFS), a scalable distributed file system for large distributed data-intensive applications.

## Motivations

There are a number of key observations of Google's application workloads and technological environment that reflect a marked departure from some earlier file system design assumptions.

1. Component failures are the norm rather than the exception.

2. Files are huge by traditional standards.

3. The workloads primarily consist of two kinds of reads: large streaming reads and small random reads. The workloads also have many large, sequential writes that append data to files.

4. The system must efficiently handle concurrent appending to the same file.

5. High sustained bandwidth is more important than low latency.

## Solution

This paper presents the Google File System (GFS), whose design is guided by the above assumptions under Google's application workload and technological environment.

### Architecture

A GFS cluster consists of a single master (holds file system metadata, and manages chunk replicas throughout the system) and multiple chunkservers (hold replicated data and respond to client requests).

### Large Chunk Size

Files are divided into chunks. A large chunk size (64 MB) is chosen to reduce client-master interaction, network overhead, and size of metadata, while increasing the chance of having hot spots in the system.

### Relaxed Consistency Model

GFS has a relaxed consistency model with the following guarantees: file namespace mutations are atomic, and after a sequence of successful mutations the file region is defined and contain the data written by the last mutation.

### System Interactions

The system is designed to minimize the master’s involvement in all operations. The master grants a chunk lease to one of the replicas selected as the primary replica, which minimize management overhead at the master. During the order decision process, data flow and control flow are decoupled to fully utilize each machine's network bandwidth. Also, GFS supports efficient snapshot and record append operations.

### Master Operation

GFS use read-write locks to manage namespaces.  To maximize system scalability, reliability, and availability, GFS adopts smart methods to do replica placement, re-replication, rebalancing, and garbage collection (lazily remove unused files).

### Fault Tolerance

Both the master and the chunkserver are designed to restore their state and start in seconds no matter how they terminated. Each chunk is replicated on multiple chunkservers on different racks. The master logs file mutations to an operation log stored on its local disk and replicated on remote machines. Each chunkserver uses checksumming to detect corruption of stored data.

## Evaluation

The authors use two real-world sample workloads to illustrate their design assumptions are correct, and the design choices meet needs.

## Takeaways

In one sentence: observe workloads, extract assumptions, then do optimizations accordingly.

For example, GFS's target workloads have little reuse within a single application run because they either stream through a large data set or randomly seek within it and read small amounts of data each time, so it does not use any caching. The target workload have a number of large files, so GFS uses a large chunk size. Google uses commodity hardware that are not quite reliable, so a lot of replication designs are in place to provide reliability.
