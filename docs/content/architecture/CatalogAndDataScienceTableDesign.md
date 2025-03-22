## ADR - Table Design for Catalog and Recommendation Items

### Problem Statement: How can we join data from to different sources; datascience recommendations and catalog data (shows/episodes).
We need performant way to surface details about recommendations to users which can
handle time sensitive content programming.

Options:
1. Single table architecture for both Recommendation and Catalog Datasets.
2. Two table solution not joining data during ingestion:
Recommendations table with profile_id focused indexes for queries.
Catalog table with asset_id (show/episode/other content) focused indexes for queries.
Services will then need to join the 2 datasets either in RT.
3. Single or Two table solution joining data during ingestion:
Recommendations table with profile_id & asset_id focused indexes for queries. During
when records are inserted to this table catalog attributes are also fetched and enter
with each item. This eliminates the need to merge data in RT at the cost of highly
denormalized data which will need to be kept in sync.
Catalog table with asset_id (show/episode/other content) focused indexes for queries.
CDC will update denormalized data in Recommendation table.

Proposed Solution:
We believe solution number 2 will be the simplest to implement and maintain while
providing necessary performance.


### Tools we can use for table design
#### Use of GSIs
DynamoDB global secondary index is a type of index containing a partition key and a sort
key different from the base table's primary key. It is known as the "global" secondary
index since the queries on the index can access data from multiple partitions of the base
table. GISs support non-unique attributes, increasing query flexibility by allowing to
run queries against non-key attributes.
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html

#### Use of LSIs
DynamodB local secondary index when you need to support querying items with different
sorting order of attributes, but the same partition key.
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/LSI.html

#### Adjacency List
When different entities of an application have a many-to-many relationship between them,
it is easier to model the relationship as an adjacency list. In this model, all top-level
entities (synonymous with nodes in the graph model) are represented as the partition key.
Any relationship with other entities (edges in a graph) are represented as an item within
the partition by setting the value of the sort key to the target entity ID (target node).

#### Materialized Graph
Materialized graph pattern is not so commonly used. It is the evolution of the adjacency
list to a more complex pattern. You do not store just relationships but also define the
type of relationship and hierarchy of the data. It is useful when you want to store a graph
structure, for example, for a modern social networking app. It is a combination of all the
relationship patterns. You store connections and type of connection between nodes. Then you
play with sort keys and GSI to see the other part of the relationship and achieve other
access patterns.

https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-adjacency-graphs.html

### Tips for optimal DynamoDB Table Desigin
https://www.serverlesslife.com/DynamoDB_Design_Patterns_for_Single_Table_Design.html
