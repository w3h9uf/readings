# Chapter 1

## DBMS major categories: 
- Online transaction processing (OLTP) databases
- Online analytical processing (OLAP) databases
- Hybrid transactional and analytical processing (HTAP)

![DBMS Architecture](image/DBMS_architecture.png)

The main limiting factors on the growth of in-memory databases are RAM volatility (in other words, lack of durability) and consts. 

### Row-Oriented / Column-Oriented DBMS
Row-oriented store: MySQL, PostgreSQL
Column-oriented store: MonetDB, C-Store

Wide-column stores -- BigTable, HBase

### Data Files and Index Files
Data Files:
- **index-organized tables** (IOT) store data in the index itself. order by key.
- **heap-organized tables** (heap files). Records are not in particular order, placed in write order. needs additional index structure
- **hash-organized tables** (hashed files) store data in the buckets by hash value of the key.

Index files:
- primary index is actually the primary key or a set of keys identified as primary. 
- secondary indexes can point directly to the data record, or simply store its primary key. (There are pros and cons for both of them. pointing directly can save look up time, but need to maintain the pointer if data record gets changed. While store primary key will have overhead on look up, but saves pointer maintainence. 
- primary indexes are most often clustered, while secondary indexes are nonclustered by definition, since they are used to facilitate access by keys other than the primary one. 

### The three common variables for various storage structures
- **buffering (or not)** defines whether or not the storage structure choose to collect a certain amount of data in memory before putting it on disk;
- **immutable (or mutable) file** defines whether or not the storage structure reads parts of the file, updates them, and writes the updated results at the same location in the file. Immutable structures are *append-only*
  - *copy-on-write* means modified page, holding the updated version of the record, is written to the new location in the file
- **store value in order (or out of order)** defines as whether or not the data records are stored in the key order in the pages on disk. Ordering often defines whether or not we can efficiently scan the range of records. However storing data out of order opens up for some write-time optimization.

