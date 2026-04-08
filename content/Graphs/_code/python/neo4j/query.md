# Run a query on a Memgraph DB

```python
from typing import LiteralString
from neo4j import GraphDatabase

def get_graph_data(memgraph_uri: str, query: LiteralString, parameters1):
    with GraphDatabase.driver(memgraph_uri) as client:
        # Check the connection
        client.verify_connectivity()

        records, _, _ = client.execute_query(
            query,
            database_="memgraph",
            parameters_={
                "parameters1": parameters1,
            },
        )

        return [
            {
                "ret1": r["ret1"],
            }
            for r in records
        ]
```
