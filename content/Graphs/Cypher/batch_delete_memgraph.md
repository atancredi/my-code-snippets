# Batch deleting in Memgraph

### Query to delete all nodes
```
USING PERIODIC COMMIT num_rows
MATCH (n:Node)
DETACH DELETE n;
```


### Query to delete nodes labeled with Node
```
USING PERIODIC COMMIT num_rows
MATCH (n:Node)
DETACH DELETE n;
```

### Query to delete relationships of type :REL
```
USING PERIODIC COMMIT num_rows 
MATCH (n)-[r:REL]->(m)
DELETE r;
```

### Query to delete all relationships:
```
USING PERIODIC COMMIT num_rows 
MATCH (n)-[r]->(m)
DELETE r;
```

Choose the num_rows which is most suitable for the amount of RAM you have available, based on the calculations we provided before. If the deletion still consumes too much memory, consider lowering the batch size.

[source: Memgraph Docs](https://memgraph.com/docs/querying/read-and-modify-data#batching-deletes)
