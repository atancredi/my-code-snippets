# Run Memgraph locally with Docker

### 1. Create a `Dockerfile` in the root folder
```dockerfile
FROM memgraph/memgraph-mage:latest
USER root

RUN chown -R memgraph:memgraph /usr/lib/memgraph/
USER memgraph
```


### 2. Create the `docker-compose.yaml` file in the root folder
```yaml
services:
  memgraph:
    build: .
    container_name: memgraph-mage
    pull_policy: always
    ports:
      - "7687:7687"
      - "7444:7444"
    command: ["--memory-limit=4096","--query-modules-directory=/usr/lib/memgraph/query_modules,/usr/lib/memgraph/custom_query_modules","--log-level=TRACE"]
    volumes:
      - memgraph-query-modules:/usr/lib/memgraph/custom_query_modules
      - memgraph-graph-data:/var/lib/memgraph
      - memgraph-logs:/var/log/memgraph
      - memgraph-conf:/etc/memgraph

  lab:
    image: memgraph/lab:latest
    container_name: memgraph-lab
    pull_policy: always
    ports:
      - "3000:3000"
    depends_on:
      - memgraph
    environment:
      - QUICK_CONNECT_MG_HOST=memgraph
      - QUICK_CONNECT_MG_PORT=7687

volumes:
  memgraph-query-modules:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/<path>/query_modules'
  memgraph-graph-data:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/<path>/persistence'
  memgraph-logs:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/<path>/logs'
  memgraph-conf:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/<path>/conf'

```

### Run with `docker compose up`