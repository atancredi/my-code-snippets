# Useful queries

## Memory modes

```
STORAGE MODE IN_MEMORY_ANALYTICAL;
STORAGE MODE IN_MEMORY_TRANSACTIONAL;
```
[Different STORAGE MODES - Memgraph Docs](https://memgraph.com/docs/fundamentals/storage-memory-usage#check-storage-mode)

## Drop all graph's content
```
STORAGE MODE IN_MEMORY_ANALYTICAL;
DROP GRAPH;
```


## Transactions

### Show current transactions
```
SHOW TRANSACTIONS;
```

## Snapshots

```
CREATE SNAPSHOT;
SHOW NEXT SNAPSHOT;
SHOW SNAPSHOTS;
```
[Memgraph Snapshots](https://memgraph.com/blog/memgraph-snapshots-explained)

[Memgraph backup and restore](https://memgraph.com/docs/database-management/backup-and-restore)


## Modules and procedures
#### reload all modules
```
CALL mg.load_all();
```

#### reload specific module
```
CALL mg.load("<module>");
```

#### show all procedures from all modules
```
CALL mg.procedures() YIELD *
```
