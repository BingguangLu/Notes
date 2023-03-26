# 1 Introduction

## 1.1 Motivation

All successful companies and organizations rely on the effective and efficient manipulation of data stored in computers

Increasingly this data:

- Includes more and more aspects of bussinesses
- Getting larger and larger
- More complex and includes novel data types such as images, etc
- Stored in various sites and accessed by many users

Historically, companies stored their data across a few files, but this eventually became inadequate:

- As data grows one or many files gets cumbersome to store data
- As users who access it grow it becomes hard to coordinate access
- As systems grow, it is hard to guard against loss, format differences, breach or damage to data...

Thus: Came the Database Era

Eventually people developed the concept of Databases which is at the base of many systems that store data today

A database system thus provides

- Secure and reliable storage of data
- Ability to retrieve and process the data effectively and efficiently

## 1.2 Subject Content Ideas

We will cover:

- Knowledge of how DB systems work at the architectural level
- Essentials to achieve correct behaviour and the best possible performance
- Mechanisms used by current systems to provide many useful features
- Techniques for achieving reliability and good concurrency among other things

## 1.3 Where to Start Studying Advanced Databases

Recall what you already know first:

- You have seen how to organize data for the most popular DBMS type
  - Relational Databases: for these you know creation of tables
- You have run queries and populated data into tables as well
  - Thus you know how to ren SQL queries
- Databases basically created a nice and tidy way to deal with data for you already

Now it is time to see under the hood:

- The cost of structure and tidiness came with a complexity associated with accessing data efficiently, this is under the hood, e.g., not putting everything in one file means a lot of join operations
- If one wants to create a generic systen to store and query large data in a tidy way, then one needs to forget about specialized fast programs and files but still one needs to compete in terms of efficiency by inventing novel methods
- This means database researchers have invested a lot of time creating under the hood thechniques for performance
- Understanding the key costs and considerations in the whole process is a good point to start looking at advanced topics

## 1.4 Performance of a Database System Comes From

Hardware

- The speed of the processor
- Number of precessors
- Number of drives and I/O bandwidth
- Size of main memory
- Communication network
- Type of architecture

Software

- Type and details of database technology used for a given application

Database tuning, crash recovery

- Indexing parameters
- Data duplication
- Sharing data
- ...

### 1.4.1 Disks

- Tracks: circular path on disk surface
- Tracks are subdivided into disk sectors

### 1.4.2 Modellig Disk Access

Disk access time = seek time + rotational time + $\frac{\text{transfer length}}{bandwidth}$

### 1.4.3 New Disks (SSD)

Solid-State Drive (SSD):

- No moving parts like Hard Disk Drive (HDD)
- Silicon rather than magnetic materials
- No seek/rotational latency
- No start-up times like HDD
- Runs silently
- Random access of typically under 100 micro-seconds campared 2000 - 3000 micro-seconds for HDD
- Relatively very expensive, thus did not dominate at all fronts yet... but soon will be... nevertheless did not effect DBMS when they were born
- Certain read/write limitations plagued it for years
$$
\text{Disk access time } = \frac{\text{transfer length}}{bandwidth}
$$

### 1.4.4 CPU Clocks

Moore's law: memory chip capacity doubles every 18 month since 1970

Joy's law for processors: processor performance doubles every two years since 1984

In general, in database system implementations, hardware limitations effect how things are done and this changes over decades, slowly but surely...

### 1.4.5 What Does This Mean?

Harddisk was the foundation stone for many DBMS design choices under the hood, and this applies even today

In future more designs choices for databases will be made based on SSD and CPU problems and these will likely dominate the arena eventually

Another recent player is networking:

- More and more databases are distributed
- The network hardware speeds are at the speed of light already
- The network router firmware/software forms the bottleneck
- Can this be another determining front for DBMS design choices
- Some of these are already at play in various new systems!

### 1.4.6 Menory Hierarchy

Data is in a memory hierarchy

This leads to a more complicated model

$$
\text{Hit ratio } = \frac{\text{references satisfied by chache}}{\text{total references}}
$$

Effective memory access time: $EA = H*C + (1 - H)*M$ where:

- H = hit ratio
- C = cache access time
- M = memory access time

## 1.5 Storage

Storage today does not come as one disk as well

They come as a system and increasingly complex

Storage systems can determine the performance and also fault tolerance of a Database

In Database Management Systems (DBMSs), rarely data is stored in one/local location and rather available over a network

Storage systems are now much larger, involves many disks, and accessd over a network, and at many sites

### 1.5.1 Storage Area Networks (SAN)

A dedicated network of storage devices

Storage can be organized as RAID (Redundant Array of Inexpensive Disks) systems

Storage is partitioned and allocated to each system and can also be shared

- They are used for shared-disk file systems
- They regularly also allow for automated back up functionality
- It was the fundamental storage for data center type systems with mainframes for decades
- Different versions evolved over time to allow for more data but fundamentals are the same even today
- They came with their own networking capabilities
- We visit a version of these when we see what can be done about failures
- In a nutshell failure probability of one disk is different to 100s of disks which requires design choices

### 1.5.2 Communication Costs

When things are stored afar then increasingly, another item to model is the cost of data transfer/communication

$$
\text{transmit time } = \text{ distance}/\text{c} + \text{message bits}/\text{bandwidth}
$$
where $c$ is the speed of light

Today it is hard to reduce hardware related latency on contemporary systems further

The key cost in latency comes from software in networking

The DBMS has little control over that

So each message length should be large to achieve better link utilzation while keep the overall need to transfer data to a minimum

## 1.6 Key DBMS Types

- Simple files
  - As a plain text file. Each line holds one record, with fields separated by delimiters
- RDBS
  - As a collection of tables (relations) consisting of rows and column. A primary key is used to uniquely identify each row.
- Object oriented
  - Data stored in the form of 'objects' directly (like OOP)
- No-SQL
  - Non relational - database modelled other than the tabular relations. Covers a wide range of emerging databases

### 1.6.1 Simple file(s)

- Usually very fast for simple (or sometimes specialized) applications but can be slow for "complex" applications
- Can be less reliable
- Application dependent optimisation
- Very hard to maintain them
- Concurrency problems
- Many of the required features (that exist in relational databases) need to be incorporated - unnecessary code development and potential increase in unreliability
- Eventually were left behind for most business cases of today
- Some specialized cases, e.g., scientific data, may use them

### 1.6.2 Relational DB Systems

- Very reliable in terms of consistency of data
- Application independent optimisation, so relatively fast already
- Well suited to more and more applications, increasingly very fast due to large main memory machines and increasing use of SSDs
- Some RDB also support Object Oriented model e.g., Oracle, DB2, and XML data+queries
- Can be slow for some special applications still

#### Transaction

A transaction is a collection of operations that need to be performed as a complete unit

ACID properties:

- Atomicity: A transaction’s changes to the state (Database) are atomic implying either all actions happen or none happen.
- Consistency: Transaction are a correct transformation of the state. Actions taken as a whole by a transaction do not violate the integrity of the state. That is we are assuming transactions are correct programs.
- Isolation: Even when several transactions are executed simultaneously, it appears to each transaction T that others executed either happen before T or after T but not at the same time. Concurrent execution of transactions should not violate consistency of data.
- Durability: State changes committed by a transaction survive failures.

Despite adversarial conditions DBMS should/will:

- Fast access to large amounts of data
- Provide a secure and stable repository when things fail
- Provides standard interfaces to data definition and manipulation with transactions
- Help multiuser accesses are done in an orderly manner but efficiently as well
- Allow convenient ways for report production and browsing
- Ease in loading data, archiving, performance tuning

#### Common System Lifecycle

For example: HDDs have about 10 years to MTTF. If you have 360 HDDs in a SAN (assuming independent failures at uniformly random distribution), then one failure about every 10 days. Assuming it takes 1 day to replace the failed unit use the following:

Module availability: measures the ratio of service accomplishment to slapsed time = 

$$
\frac{\text{Mean time to failure}}{\text{Mean time to failure + mean time to repair}}
$$

$10/11$ or about $91\%$ availability for your system. For a bank system your aim is to achieve levels like $99\%+$... How does DBMSs do that?

### 1.6.3 Object Oriented (OO) DB Systems

- Stores as objects directly, not tables
- May contain both data (attributes) and methods – like OOP
- Can be slow for some applications
- Reliable
- Limited application independent optimisation
- Well suited for applications requiring complex data
- Unfortunately, many commercial systems started this way did not survive the force of RDB technology and basically disappeared from the market
- A take away message: do not throwaway your RDBs yet, they appear to be very resilient!

### 1.6.4 NoSQL

- Flexible/ no fixed schema (unlike RDB)
- Provides a mechanism for storage and retrieval of data modelled in means other than the tabular relations, used with BigData
- Simple design, should linearly scale
- NoSQL has compromised consistency and allows replications - We will discuss this more later -> leads to supposedly faster exec
- Most NoSQL databases offer "eventual consistency”, which might result in reading data from an older version, a problem known as stale reads – we will learn more on these later as well

#### Types

- Key-Value
  - Many applications do not require the expressive functionality of transaction processing and rigidity
  - Used for building very fast, highly parallel processing of large data - Hadoop is another example here
- Document-Based
- Column-Based
- Graph-Bases

## 1.7 Taxonomy

- Centeralized
  - Data stored in one location
- Distributed
  - Data distributed across several nodes, can be in different locations
- WWW
  - Stored all over the world, several owners of the data, litte organization
- Grid
  - Similar to Distributed, but each node manages own resources but there is some structure
- P2P
  - Like Grid, but nodes can join and leave network at will
- Cloud
  - Generalization of Grid, but resources are accessed on-demand, run by a company

## 1.8 Database Architectures Continued

### 1.8.1 Centeralised

Originally we had: Centralised database systems

- Data in one location
- System adminstration is simple
- Optimisation process is generally very effective on simple transactions

Many centralized systems use a Client-server architecture as well

- A central server with a central DB in one location
- Client generally provides user interfaces for input and output
- Server provides all the necessary database functionality
- System administration is relatively simple
- System recovery is basically same as previous case, i.e., simple

### 1.8.2 Distributed Database Systems

- Data is distributed across several nodes in different locations
- Nodes are connected by network
- System provides very advanced concurrency, recovery and other transaction processing capabilities
- System administration is very hard
- Crash recovery is complicated
- There is usually data replication - adds more inconsistency issues

### 1.8.3 World Wide Web

- Data is stored in many many locations
- Several owners of data - no certainty of data availability or consistency
- Optimisation process is generally very ineffective
- Dream database technology - no standards have been developed except in case of XML/http and some protocols for accessing data
- Security would be a problem
- Notions of transactions is much more difficult to enforce or non-existent

### 1.8.4 Grid Computing and Databases

- Very similar to distributed database systems
- Data and processing are shared among a group of computer systems which may be geographically separated
- Usually designed for particular purpose - e.g., a scientific data management, and the grid is dedicated to that purpose
- Administration of such systems are done locally by each owner of the system but there is some protocol/structure
- Reliability, security etc of such systems are not well developed or studied
- Grid systems became quickly outdated and overwritten by the Cloud Computing model

### 1.8.5 P2P Databases

- Data and processing is shared among a group of computers which are commonly geographically separated and form a large set
- Computer nodes can join and leave the network at will unlike in Grid Databases -> much harder to design transaction models
- Usually designed for particular usage - e.g. a scientific application, by citizen scientists
- Administartion of such system is done by the owners of each PC
- An idea that keeps on coming back

### 1.8.6 Cloud Computing-Based DB Systems

Cloud computing offers online computing, storage and a range of new services for data and devices that are accessible through the Internet

User pays for the services as they go just like phone services, electricity,etc

Large potential with minimal infrastructure costs (this is a hot area of research from many angles and has taken off well)

Dynamicity of the resources is a challenge for transactional models Cloud services are offered in several forms:

- IaaS – Infrastructure as a service (provide virtual machines)
- PaaS – Platform as a service (Provide environment like Linux, Windows, etc.)
- SaaS – Software as a service (also known as DBaaS where DBMS is there already)
