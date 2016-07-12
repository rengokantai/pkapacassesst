#### pkapacassesst
#####Chapter 3. Creating Database and Schema
######A database and schema
partition keys and clustering columns
```
primary key(a,b)   a:pk b:cc
primary key((a,b),c) a,b:composite pk c:cc
primary key(a,b,c) a:pk b,c:cc
```
static columns(expect same value for this column)
```
prop text static
```
######Secondary indexes
Indexes should not be created on columns that have unique values for most of the rows.(low cardinality.)  
If we require efficient queries on high cardinality columns we should consider performing Denormalization

######Conditional querying
Only equality or IN relations are allowed on the queries based on the partition key.   
Also, IN relations are only allowed on the last part of the partition key. 
######Batch statements
```
BEGIN BATCH
APPLY BATCH;
```
```
BEGIN UNLOGGED BATCH
APPLY BATCH;
```
#####Chapter 7. Backup and Restore
######Restoring data to Cassandra
Delete the old commitlog files:
```
rm /var/lib/cassandra/commitlog/*
```
Delete the old SSTable
```
 rm /var/lib/cassandra/data/db/table-123/*.db
```
copy snapshot to data directory
```
cp –p /var/lib/Cassandra/data/db/table-123/snapshots/1234567/* \
/var/lib/Cassandra/data/db/table-123/
```
then
```
nodetool repair
```
bulk load-- sstableloader
```
sstableloader -d 1.2.3.4 /data/db/table
```
######Removing nodes from Cassandra cluster
if a node is down,
```
nodetool removenode 12345678-1234-1234-1234-123456781234
nodetool removenode status
```
if a node is not down
```
nodetool decommission
```
######Replacing dead nodes in a cluster
same as before
```
nodetool removenode
nodetool decommission
```
