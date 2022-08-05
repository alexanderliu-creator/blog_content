---
title: MIT分布式系统
date: 2021-09-24 19:45:20
tags: 大三自学
---



# 这是MIT分布式系统相关课程

# Introduction

- Performance：计算机的表现：
  - Scalability
  - Fauct Tolerance
- Scalability：可拓展性
  - Scalability ---> 2x computers ---> 2x throughput ， 如果我们拥有两倍以上的计算机，我们能够拥有对应倍数的性能，这个就叫做可拓展性。

![image-20210924201118810](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924201127.png)

Maybe like this , we have many web servers . With the growth of the web servers. The web servers are not longer the bottleneck , as we can see , we only have one db in the whole picture. The db becomes the bottleneck now ! This way is not **scalable**. 

- Fault Tolerance：故障容忍性
  - Availability：可用性，当所有机器没有全挂的时候，应该保证用户的可用性。
  - Recoverability：可恢复性，例如故障了，如果我人为修复了它，它是否能够继续可用？晚上的网络故障我早上修复好，早上还可以继续使用，没有任何问题。这就体现了高可恢复性。
    - NV Storage
    - Replication
- Consistency：
  - Put operation：`put(key,value)`
  - Get operation：`get(key) -> value`

The distribution system has many versions ,  we should try to keep the consistency of different computers.

Strong consistency --- Weak consistency

Weak dosen't guarantee you can see the newest data.

Readers and writers should communicate many times to implement strong consistency to let every duplication to get the most recent value they find.

Expensive -> to avoid communication -> Weak system , allow reading old values ,

People like to put different replicates as far apart as possible , like in different cities to construct different data centers. But !!! It takes time to communciate to different replicates , so it's **expensive** for us to build the strong consistency system.



## MapReduce

- Google designed and built and used the MapReduce

Consuming inputs and split

![image-20210925114653238](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925114653.png)

Entire job -> Map tasks

- A esay implement：

![image-20210925144755568](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925144755.png)

- GFS：
  - Google File System
  - It's a cluster file system
  - Input are stored in the GFS , MapReduce automatically split the inputs and let different computers to execute the tasks. The Reduce will collect the different answers from corresponding servers.
  - Communciation through network is also a big challenge for the whole system
  - Every computer run



# RPC  and Threads

## Go

- GC and safety multi threads and safety allocation of memories make it easy to learn and do our experiment. It is an easy language.

## Thread

- Threads are main tools to manage concurrency. Threads are equal to `go routines` in go.
- Reasons for tho importance of multi thread in distributing system:

  1. I/O concurrency

  2. Parallelism

  3. Convenience:
     - Just like if you want to do a periodical job, you can let a thread to do this job periodically.
     - The job is like checking if the workers(In the C/S model) is still alive . If some parts die , we will lift up another worker in another machine



- Actually go doesn't know anything about the relationship between lock and variables. So you should schedule where the lock is used to protect our codes.



- Coordination of multi threads：
  - Channels
  - Sync.Cond
  - WaitGroup
- Dead Lock



# GFS

- The Google File System
- Why  is it hard?
  - Performance -> Sharding
  - Faults -> Tolerance
  - Tolerance -> Replication
  - Replication -> Inconsistency
  - Consistency -> Low Performance

- Bad Replication Design
  - two servers

![image-20210927193742870](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210927193743.png)

we can't gurantee the order of the proccess execute.（which is also called anomalous behaviors）

- GFS：
  - How to fix the model below?
  - Demands:
    - Big
    - Fast
    - Sharding
    - Automatic recovery
  - Features:
    - Internal use
    - Big sequential access

![image-20210927195450882](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210927195451.png)

Write has to go to the disk , Read can from the memory to make the data safe . Log and checkpoints should all be kept in disk.

Master knows where to find the data , so it can tell to go to which chunk !!!

## Read operation:

1. name , offset -> master (client only thinking about the filename and the offset , the master is responsible to find the data maybe from several seperate servers and return the data to the client)
2. master -> chunk handler and list of server (maybe depend on distance , The master will get a list of servers which hold the chunk of the file , and select one , maybe by distance , and let client communicate with that server). Cache the result. (Find which server that hold the chunk of the file)
3. Client -> One of the chunk servers
4. Servers -> Return the data to client

- So one question i consider is that you're actually reading **replicas** of data , maybe there are several servers hold the chunk , and you only communicate with one of them which maybe results in inconsistency of data.



## Write operation:

- When the master reboots , it should communicate to each server to find which server holds which chunk.
- One case is not primary:
  - Find up to date replicas
  - Pick up primary , secondary servers
  - Increments version numbers
  - Tells primary , secondary versions , and give them leases
  - Master writes version to disk
  - Primary picks offset all replicas and told to write at off
  - If all secondary server say "yes" , primary say "yes" to client. Else say "no" to client. (Maybe some of the secondary servers successfully do the job , primary may still reply "no" to client , that is how it works!!!)
- all the secondary servers are the same versions . Only when the master signs the new primary , the version changes（就像我们学习的读服务器和写服务器，写服务器会将内容写到每一个读服务器，以保证数据的一致性一样的嗷！！！，主从复制orz）
- 当primary服务器群数据不一样的时候，就有可能根据谁的版本最新，进行服务器之间的通信，来更新所有的服务器到最新的版本。（但实际上这种行为不一定会发生，所有secondary的版本都是一样的嗷！！！当出现上述的情况的时候，服务器会尝试先自己进行修复嗷！！！）

![image-20210927205746538](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210927205747.png)

Actually we can ensure high concurrency by like waiting for the right operation for every server or redo the task , but !!! That will lower the availability.

- When a primary died , the master will desiganate a new one. But the master must wait for the expiration of the lease.
- Always three replicas a for chunk for default , always one primary and two backups.
- `"SPLIT BRAIN"` is the net partition phenonmenon. Hardest problem to deal with. 
- If a primary failed to communicate with the master , the master will sit down to wait for the expiration of the lease and it will maybe designate another "brain" or primary to complete the mission.
- How to update GFS?
  - Need a primary to detect duplicated requests
  - When the disks are wrong , and maybe the secondaries are down , what should we do to take away the broken ones and ensure the system is running
  - When primary asks secondaries to append something , secondaries should not expose their data to readers until the primary ensure the all secondaries really need to execute the append.
  - If primary crahes ? New primary may differ from the operations of the old secondaries. New primary  should be resynchronizing with the secondaries to make sure they have the same operation history.

- The most serious limitation is that we have only one single master.





# Primary-Backup Replication

- Faliures：
  - Fail-stop faults
  - Bugs

- State Transfer
  - Copy the contents of RAM of the primary , and send them to the backup. Maybe you can send the memory changed from last time more efficiently . This is actually what mysql does.
  - Transfer memory
- Replicated state Machine
  - don't send the state among the replicas , just send the external events from the outside input.
  - Transfer operations from clients or external events
- people tend to choose replicated state , because operations are always smaller.
- Maybe the secondary will take the job if the master is dead , because the secondary may has the latest state of the primary computer.



- What do we mean about state



















