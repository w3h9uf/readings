## Reliable, Scalable and Mantainable Applications

### Scalability
Service Level Agreement
Service Level Objective
### accidental complexity
> Complexity is _accidental_ if it is not inherent in the problem that the software solves but arise only from the implementation

: next time you want to make it fancy, think about this. Is this solving the original problem that related to user? Or it's just your extension

## Data Model and Query Languages

- NoSQL -> Not Only SQL
- Object-relational mapping (ORM)
- Hierarchical models (e.g. JSON model, Document-based Model) are good for one-to-many relationship, but not suitable for many-to-many relationship (e.g. common friends, common schools)
- To solve many-to-many relationship -> relational model(SQL) and Network Model(obseleted)

### Relational v.s. Ducument DB
Ducument
- schema flexibility
- better performance due to locallity
- closer to the data structures used by the applications

Relational
- better support for joins
- many-to-one and many-to-many relationships

> If application does use many-yo-many relationships, the document model becomes less appealing
> For highly interconnected data, the document model is awkward, the relational model is acceptable, and graph models are the most natural

__query locality__: grouping related data togather
`Spanner, Cassandra, HBase`

### Query Languages for Data
- _declarative_ query languages (SQL)
- _imperative_ code (IMS, CODAYSYL)

> In declarative language, you just specify the pattern of the data you want, but not how to achieve that goal. It is up to the database system's query optimizer to decide which indexes and which join methods to use.

- MapReduce Query: neither declarative nor imperative, something in between (MongoDB has MR support)

### Graph-Like Data Models
social graphs, web graphs, road or rail networks 
- Property Graph Model (Cypher)
- triple-store model (SPARQL) -> triple is for _subject, predicates, object_

> Graphs are good for evolvability

- The Foundation: Datalog


## Storage and Retrieval
- _log-structured_ storage engines
- _page-oriented_ storage engines

### Log-structured
usually append only operations
- write is faster
- concurrency and crash recovery are much simpler

hash key is in-memory mapping
- need to handle crash recovery
- good for data with not so many keys, but become expensive when number of keys are large

Sorted String Table (SSTable)
- items in segment are sorted by key
- merging segment is just _merge sort_
- write will first go to in-memory map, then stored to disk segment. So crash recovery can be an issue
- `LevelDB, RocksDB, Cassandra, HBase`
- first introduced in Google's BigTable

### B-Trees
- most widely used

> One _page_ (usually 4kb or more) is designated as the root of the B-tree. The page contains serveral keys and references to child pages. Each child is responsible for a continuous range of keys, and keys between the references indicate where the boundaries between those ranges lie.

- _branching factor_, _levels_
- update/add keys, find the key, update page, if no more space in page, split page into two (then you need also update parent ref page, which can cause inatomic operation)
- crash recovery: a _write-ahead log_, append-only file showing the operation history

B-Trees v.s. LSM-Trees
- LSM-Trees faster for write, B-Trees faster for read


### In-memory database
- crash recovery from replica or disk
- `Memcached, VoltDB, MemSQL, TimesTen, RAMCloud, Redis, Couchbase`
- In-memory database's performance advantage is not due to the fact they don't need to read from disk. Disk-based storage engine may never need to read from disk if you have enough meomory, because the operating system caches recently used disk in memory anyaway. Rather, they can be faster because they can avoid the overheads of encoding in-memory data structures in a form that can be written to disk.


### Transaction processing / analytic processing
analytic processing - data warehouse


## Encoding and Evolution
- backward compatibility: new code can read data that is written by old code
- forward compatibility: old code can read data that is written by new code

- JSON, XML, CSV
- JSON is less verbose than XML, but both still use a lot of space compared binary formats

### Binary encoding
- `Thrift (Facebook), Protocol Buffers (Google)`
Thrift
```
struct Person {
  1: required string username;
  2: optional i64 favoritesNumber;
  3: optional list<string> interests;
}
```
Protocol Buffers
```
message Person {
  required string user_name = 1;
  optional int64 favorite_number = 2;
  repeated string interests = 3;
}
```

- `Avro`


### Modes of Dataflow
#### Dataflow through database
- backward compatibility
- old code should leave the new field inact


#### Dataflow through service calls 
- REST and RPC
- _web services_: HTTP is used as the underlying protocol for talking to the service
- _middleware_: one service making requests to another service owned by the same organization, often located within the same datacenter, as part of a service-oriented/microservices architecture
- two approaches to web services: REST and SOAP

> REST is not a protocol, but a design philosophy that builds upon the principles of HTTP. It emphasizes simple data formats, using URLs for identifying resources and using HTTP features for cache control, authentication, and content type negotiation. REST has been gaining popularity compared to SOAP, at least in the context of cross-organizational service integration, and is often associated with microservices. An API designde according to the principles of REST is called _RESTful_

RPC (Remote Procedure Call)
- tries to make a request to remote network service look the same as calling a function or method in your programming language, within the same process.
- some flaws, local function call cannot control network processes or got useful feedback from network
- main focus of RPC frameworks is on requests between services owned by the same org, typically within the same datacenter.
- If we assume servers will be updated before clients, RPC only need to achieve backward compability for reqeusts, and forward compability for response

#### Dataflow through async message passing


## Replication
_Shared-Nothing Architectures_: also known as _horizontal scaling_
> In horizontal scaling, each machine or virtual machine running the database software is called a node, Each node uses its CPUs, RAM, and disks independently. Any coordination between nodes is done at the software level, using a conventional network.


Three popular algo for replicating changes between nodes: _single leader_, _multi-leader_, _leaderless_

### leader and followers model
- sync v.s. async
- you cannot go pure sync, since a node failure will make the whole system down, usually use sync means make one follower sync, other followers are async
- setup new follower: take a snapshot of leader without blocking the DB, copy to new follower, start follower and request for lost data after snapshot
- Handling Node Outage
  - Follower failure: catch-up recovery
  - leader failure: failover (one follower be promoted to leader, start handling client request), steps:
    - determine that the leader has failed
    - choose a new leader
    - reconfigure the system to use the new leader
  - some caveats:
    - for async replication, new leaders may not have recieved all writes from the older leader before it failed. The new leader may recieve conflict writes, we have to discard those unreplicated writes, which is dangerous
    - two leaders may be chosen (_split brain_). One leader must be shut down, (both will be shut down if not handle well)
    - trade-off of chose a leader dead timeout

### Implementation of Replication Logs
#### statement-based replication
leader sends every operations to followers to replicate
issues:
- statement with nondeterministic function (NOW(), RAND(), etc) will not work properly
- statements with nondeterministic side effects will also suffer

#### write-ahead log (WAL) shipping
> besides writing the log to disk, the leader also sends it across the nwework to its followers. When the followers process this log, they build a copy of the exact same data structures as found on the leader. 
> If followers are allowed to run newer version of software than leader, you can perform __zero-downtime__ upgrade of the database software.

#### logical (row-based) log replication

#### trigger-based replication
_triggers_ / _stored procedures_: a trigger lets you register custom application code that is automatically executed when a data change occurs in a database system

### Problems with Replication Lag
_eventual consistency_
When node is operating near capacity or if there is a problem in the network, the _replication lag_ can become fairly large (seconds or even minutes), a noticable replication lag will cause the following issue:
- reading your own writes: user made write, then read the recent write, the read is sent to a follower which has not caught up yet. To mitigate
  - when reading something that the user may have modified, read from leader
  - make replica serve any reads from that user reflects updates at least until the most recnet write
- monotonic reads: user1 writes to leader, user2 read twice, first from an updated follower, show new data, second time from another follower which is not updated yet, will cause _moving back in time_
  - make sure each user always make reads from the same follower
- _consistent prefix reads_: if a sequence of writes happens in a certain order, then anyone reading those writes will see them appear in the same order. But this guarantee is not achieved in some distributed databases

> When working with an eventually consistent system, it is worth thinking about how the application behaves if the replication lag increases to serveral minutes or even hours.

### Multi-leader Replication

- two writers write at same time, need to handle write conflicts
  - give each writes a unique ID indicating timestamp, pick the one with lastest timestamp as winner
  
- how leaders share information: _replication topology_
  - all-to-all
  - circular (tag the data with machine's unique identifier to avoid infinite replication loops
  - star topology

### Leaderless replication
also known as _Dynamo-style_

client sends writes to serveral nodes in parallel instead of only one node. May get different response but pick the majority

To make eventual consistency:
- _read repair_
- _anti-entropy process_: a process to detect discrepency in nodes


> For defining concurrency, exact time does not matter: we simply call two operations concurrent if they are both unaware of each other, regardless of the physical time at which they occurred.


## Partitions

unfiar partitioning -> _hot spot_
- Partitioning by key range
- Partitioning by hash of key
  - lose the ability to do efficient range queries like key range partitioning
  
### skewed workloads and relieving hot spots
- data systems cannot uatomatically compensate for highly skewed workload, so put the logic in application level
- if a key is very hot, put a random number to the begining or end of the key to make it evenly hashed to diffrenet partitions

### partition and secondary indexes
partioning by key, but search by secondary indexes can be awkward
- partioning secondary indexes by document 
  - each partition has it own secondary index (_local indexes_)
  - to search/query, you need to query each partition (know as _scatter/gather_) -> prone to tail lantency amplification (used in _MongoDB, Riak, Cassandra, Elasticsearch, SolrCloud, VoltDB_)
- partioning secondary indexes by term (_global indexes_)
  - _global indexes_ are partitioned into different nodes by different hash with primary key
  - reads will be more efficient, but writes can be cross partitions
  
### Rebalancing Partitions
> The process of moving load from one node in the cluster to another is called _rebalancing_
  
strategies:
- Do not use hash mode N, because if your number of partitions changed, there will a lot of unnecessary rebalacing between nodes
- create many more partitions than the number of nodes, (1000 to 10), and keep the number of partitions fixed. added nodes will _steal_ some partitions from other nodes. (the entire partition moved) (used in _Riak, Elasticsearch, Couchbase, Voldermort_)
- dynamic partitioning: number of partitions adapts to the total data volume
- partitioning proportional to nodes: have fixed number of partitions per node, added node will choose existing partions to split (_Cassandra, Ketama_)
  
### Request Routing
Q: which compenent should be responsible to route request?

A: Three possible ways:
  - node level: send to random node, node will reroute if this request not belong to it (_Cassandra, Riak_) _gossip protocol_
  - routing tier: a dedicated routing service
  - client level: expose partitions to client
  

ZooKeeper to have all nodes registered and keep nodes infos, router will keep updated by talking to ZooKeeper

  
## Transactions
> A key feature of a transaction is that it can be aborted and safely retried if an error occurred.ACID databases are based on this philosophy: if the database is in danger of violating its guaranteeof atomicity, isolation, or durability, it would rather abandon the transaction entirely than allowit to remain half-finished.

- Transaction will handle error by aborting half-finished operations, but more important is to make sure retry transient errors. 
- For transactions with side effects outside database, those side effects may happen even if the transaction is aborted. use _two-phase commit_ to make sure several different systems eother commit or abort together. 


