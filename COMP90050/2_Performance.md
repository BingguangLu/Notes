# 2 Performance

## 2.1 Query Processing

### 2.1.1 Key Hurdle: Joins

Definition:

$$
r \Join_\theta s
$$

- This operator is used to define a join of two tables on some condition, here $r$ and $s$ are tables and $\theta$ is a condition containing $<, >, =$ etc.
- This is a very common operation due to the nature of RDBs
- But also a very expensive operation
- Key in getting good performance in RDBs

SQL example of a join operation:

```SQL
Select *
From T1
Inner Join T2
On T1.a = T2.b
```

A lot of investment gone into doing an efficient join operation on various data types

It gives us a key example to see what RDBMSs do under the hood

Several different algorithms to implement joins exist that the optimizer can look at and pick the best:

- Nested-loop join
- Block nested-loop join
- Indexed nested-loop join
- Merge-join
- Hash-join
- ...

The same mentality exist for pretty much all query processing but we take joins as the key example now

#### Example

```
for each tuple t_r in r do begin
    for each tuple t_s in s do begin
        test pair (t_r, t_s) to see if they satisfy the join condition theta
        if they do, add t_r . t_s to the result
    end
end
```

- $r$ is called the outer relation and $s$ the inner relation of the join.
- Requires no indices and can be used with any kind of join condition.
- Expensive since it examines every pair of tuples in the two relations.
- Remember that for every retrieval, especially for a different item from the disk in a nonconsecutive location we pay a seek time as a penalty, this is where it could happen a large number of times.
- Could be cheap if you do it on two small tables where they fit to main memory though (disk brings the whole tables with first block access).

Lets see an example with the following bank database information:

- Number of records of customer: $10,000$ depositor: $5000$
- Number of blocks of customer: $400$ depositor: $100$

In the worst case, if there is enough memory only to hold one block of each table, the estimated cost is:

$$
n_r \times b_s + b_r
$$
block transfers, and

$$
n_r + b_r
$$
seeks

So there are two options:

With depositor as the outer relation:

– $5000 \times 400 + 100 = 2,000,100$ block transfers,
– $5000 + 100 = 5100$ seeks

With customer as the outer relation:

– $10000 \times 100 + 400 = 1,000,400$ block transfers and $10,400$ seeks
– Remember the example from last lecture: average seek time was 12 ms which was the main cost, rotation delay 4 ms, transfer rate 4MB/sec
– This would mean: $10,400 \times 12ms$ seek time which makes about $2$ mins to join two small tables (just seek time)
– If you had $1000,000$ customers like a real bank then you would wait 3 hours for one simple join (seek time only)!

Hence the better way is:

```
for each block B_r of r do begin
    for each block B_s of s do begin
        for each tuple t_r in B_r do begin
            for each tuple t_s in B_s do begin
                Check if (t_r, t_s) satisfy the join condition
                if they do, add $t_r . t_s$ to the result
            end
        end
    end
end
```

Compute time now:

- Estimate: $b_r \times b_s + b_r$ block transfers $+ 2 \times b_r$ seeks
  - Each block in the inner relation $s$ is read once for each block in the outer relation (instead of once for each tuple in the outer relation)
- Further improvements to nested loop and block nested loop algorithms exist but with the block nested loop we already get
  - $400 \times 100 + 400 = 40,400$ transfers and $800$ seeks
  - $800 \times 12ms$ is about $10$ secs which is now an order of magnitude faster than the previous option
- Thus, researchers have invented many such methods to make things run faster and reduce in particular the seek time in DBMS

## 2.2 Optimization

### 2.2.1 How Do We Make the Choices

Steps in cost-based query optimization

1. Generate the logically equivalent expressions of the query
   - An SQL query has many equivalent algebra expressions to it
2. Annotate resultant expressions to get alternative query plans
3. Choose the cheapest plan based on etimated cost with what you know about costs

Estimation of plan cost based on:

- Statistical information about tables
- Statistics estimation for intermediate results to compute cost of complex expressions
- Cost formula for algorithms, computed using styatistics again

### 2.2.2 First convert SQL to RA expression

```SQL
Select A1, A2, ..., An
From r1, r2, ..., rm
Where P
```

is same as the following in relational algebra:

$$
\prod_{A_1, A_2, ..., A_n} (\sigma_p(r_1 \times r_2 \times ... \times r_m))
$$

and such an expression can be run in many different ways/orders...

### 2.2.3 Benefits of Relational Algebra and How to Generate Alternatives?

- Query optimizers use equivalence rules to systematically generate expressions equivalent to a given expression
- SQL does not give this power as it defines what you want to get and not how you get it (good for the user but not for the machine)
- And hence relational algebra is used, which is a procedural language and deals with the “how” part
- One can generate all equivalent expressions exhaustively given a relational algebra expression
- Note that the above approach is very expensive in space and time for complex queries

### 2.2.4 Real Life

Note that:

- Must consider the interaction of evaluation techniques when choosing evaluation plans
- Choosing the cheapest algorithm for each operation independently may not yield best overall algorithm. E.g., merge-join may be costlier than hash-join, but may provide a sorted output which could be useful later
- Practical query optimizers incorporate elements of the following two broad approaches:
  1. Search all the plans and choose the best plan in a cost-based fashion.
  2. Uses heuristics to choose a plan.

Systems may use heuristics to reduce the number of choices that must be made in a cost-based fashion

Heuristic optimization transforms the query-tree by using a set of rules that typically (but not in all cases) improve execution performance:

1. Perform selections early (reduces the number of tuples)
2. Perform projections early (reduces the number of attributes)
3. Perform most restrictive selection and join operations (i.e. with
smallest result size) before other similar operations

Some systems use only heuristics, others combine heuristics with cost-based optimization

Optimizers often use simple heuristics for very cheap queries, and perform exhaustive enumeration for more expensive queries

## 2.3 Indexing

### 2.3.1 A Key Choice to Make Before Optimization

DBMS admin generally creates indicies to allow almost direct access to individual items

These indices are also good for some join operations

### 2.3.2 Critical Role

- Indexing mechanisms used to speed up access to desired data in a similar way to look up a phone book
- Search Key - attribute or set of attributes used to look up records/rows in a system like an ID of a person
- An index file consists of records (called index entries) of the form search-key, pointer to where data is
- Index files are typically much smaller than the original data files and many parts of it are already in memory
- Two basic kinds of indices:
  1. Ordered indices: search keys are stored in some order
  2. Hash indices: search keys are distributed hopefully uniformly across “buckets” using a “function”

### 2.3.3 What Gets Faster?

- Disk access becomes faster through:
  - records with a specified value in the attribute accessed with minimal disk accesses
  - or records with an attribute value falling in a specified range of values can be retrieved with a single seek and then consecutive sequential reads
- Insertion time to index is also important
- Deletion time is important as well
- No big index rearrangement after insertion and deletion
- Space overhead has to be considered for the index itself

### 2.3.4 Types

- Primary index: In a sequantially ordered file, the index whose search key specifies the sequential order of the file
  - The search key of a primary index is usually but not necessarily the primary key
- Secondary index: an index whose seach key specifies an order different from the sequential order of the file

### 2.3.5 B+ Tree

- Why need them:
  - Keeping files in order for fast search ultimately degrades as file grows, since many overflow blocks get created.
  - So binary search on ordered files cannot be done.
  - Periodic reorganization of entire file is required to achieve this.
- Advantage of B+-tree index files:
  - automatically reorganizes itself with small, local, changes, in the face of insertions and deletions.
  - Reorganization of entire file is not required to maintain performance.
- Similar to Binary tree in many aspects but the fan out is much higher
- Disadvantage of B+ trees:
  - Extra insertion and deletion overhead and space overhead
- Advantages of B+ trees outweigh disadvantages for DBMSs
  - B+ trees are used extensively

#### Definition

- It is similar to a binary tree in concept but with a fan out that is defined through a number $n$
- All paths from root to leaf are of the same length (depth)
- Each node that is not a root or a leaf has between $\lceil n/2 \rceil$ and $n$ children
- A leaf node has between $\lceil (n–1)/2 \rceil$ and $n–1$ values
- Special cases:
  - If the root is not a leaf, it has at least $2$ children.
  - If the root is a leaf (that is, there are no other nodes in the tree), it can have between $0$ and $(n– 1)$ values.

#### A Single Node

Typical node:

| $P_1$ | $K_1$ | $P_2$ | ... | $P_{n - 1}$ | $K_{n - 1}$ | $P_n$ |
| --- | --- | --- | --- | --- | --- | --- |

- $K_i$ are the search-key values
- $P_i$ are pointers to children (for non-leaf nodes) or pointers to records or buckets of records (for leaf nodes)

The search-keys in a node are ordered

$$
K_1 < K_2 < ... <K_{n - 1}
$$

#### Running a Query on B+ Trees

Finding all records with a search-key value of $k$:

1. $N = $ root initially
2. Repeat
   1. Examine $N$ for the smallest search-key value $> k$
   2. If such a value exists, assume it is $K_i$. Then set $N = P_i$
   3. Otherwise $k \geq K_{n - 1}$. Set $N = P_n$
3. If for some $i$, key $K_i = k$ follow pointer $P_i$ to the desired record or bucket
4. Else no record with search-key value $k$ exists.

#### Metrics

If there are $K$ search-key values in the file, the height of the tree is no more than $\lceil \log_{\lceil n/2 \rceil}(K)\rceil$ and it would be balanced

A node is generally the same size as a disk block, typically $4$ kilobytes

- and $n$ is typically around $100$ ($40$ bytes per index entry).

With $1$ million search key values and $n = 100$

- $\log_{50}(1000000) = 4$ nodes are accessed in a lookup

Contrast this with a balanced binary tree with $1$ million search key values - around $20$ nodes are accessed in a lookup

- above difference is significant since every node access may need a disk I/O, costing around $20$ milliseconds

#### Insertion

- If the leaf is not full
  - Insert the key into the leaf node in increasing order
- If the leaf is full
  1. Insert the key into the leaf node in increasing order and balance the tree in the following way
  2. Break the node at $n/2$th position
  3. Add $n/2$th key to the parent node as well
  4. If the parent node is already full, follow steps $2$ to $3$

#### Deletion

- The key to be deleted is present only at the leaf node not in the indexes (or internal nodes)
  - There is more than the minimum number of keys in the node:
    - Simply delete the key
  - There is an exact minimum number of keys in the node.
    - Delete the key and borrow a key from the immediate sibling. Add the median key of the sibling node to the parent
- The key to be deleted is present in the internal nodes as well.
  - If there is more than the minimum number of keys in the node
    - Simply delete the key from the leaf node and delete the key from the internal node as well. Fill the empty space in the internal node with the inorder successor.
  - If there is an exact minimum number of keys in the node
    - Then delete the key and borrow a key from its immediate sibling (through the parent). Fill the empty space created in the index (internal node) with the borrowed key.
  - If empty space is generated above the immediate parent node
    - After deleting the key, merge the empty space with its sibling. Fill the empty space in the grandparent node with the inorder successor.
- The height of tree gets shrinked.

### 2.3.6 Index Definition in SQL

Create an index:

```SQL
Create index <index-name> on <relation-name>
```

To drop an index:

```SQL
drop index <index-name>
```

Most database systems allow specification of type of index and clustering

### 2.3.7 Hash Indices

- A hash index organizes the search keys, with their associated record pointers, into a hash file structure. Order is not important.
- Hash indices are always secondary indices.
- Given a key the aim is to find the related record on file in one shot which is important.
- An ideal hash function is uniform, i.e., each bucket is assigned the same number of search-key values from the set of all possible values.
- Ideal hash function is random, so each bucket will have the same number of records assigned to it irrespective of the actual distribution of search-key values in the file.
- Typical hash functions perform computation on the internal binary representation of the search-key.

### 2.3.8 Bitmap Indices

Records in a relation are assumed to be numbered sequentially from, say, $0$

Applicable on attributes that take on a relatively small number of distinct values

A bitmap is simply an array of bits

### 2.3.9 Indexing for Complex Data Types

Unlike things we can access by names, numerical ids, etc. there is a lot of other types of data that exists and increasingly in use in DBMS and that requires special indexing

For example, spatial data requires more complex computations for accessing data, e.g., intersections of objects in space

There is no trivial way to sort items which is a key issue in that case, e.g., below is a common range query on a simple set of two items in space and a NN query

#### Solution

In 2-d space, similar to B+ trees for exponentially reducing the number of comparisons/retrievals, but via a repetitive division of space, novel indices were invented

They are in use in Oracle Spatial and other comparable DBMS extensions

The following class of data structures is one such index: Quadtrees

### 2.3.10 Quadtree

Each node of a quadtree is associated with a rectangular region of space; the top node is associated with the entire target space.

Each division happens with respect to a rule based on data type.

Each non-leaf node divides its region into four equal sized quadrents

Thus each such node has four child nodes corresponding to the four quadrants and division continues recursively until a stopping condition.

### 2.3.11 K-D tree

Each level of a k-d tree partitions the space into two

In each node, approximately half of the points stored in the sub-tree fall on one side and half on the other.

Partitioning stops when a node has less than a given number of points.

### 2.3.12 R-Trees

R-trees can be considered as a N-dimensional version of B+-trees, useful for indexing sets of rectangles and other polygonal data.

Supported in many modern database systems, with variants like R+-trees and R*-trees

Basic idea: generalize the notion of a one-dimensional interval associated with each B+-tree node to an N-dimensional interval, that is, an N-dimensional rectangle

We will consider only the common two-dimensional case (N = 2)

- generalization for $N>2$ is staightforward, although R-trees work well only for relatively small N

Disadvantage:

- Bounding boxes of children of a node are allowen to overlap

A bounding box (BB) of a node is a minimum-sized rectangle that contains all the retangles associated with the node
