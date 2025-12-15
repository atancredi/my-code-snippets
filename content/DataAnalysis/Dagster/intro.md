# introduction to Dagster

### Install
```bash
pip install dagster dagster-webserver dagster-dg-cli
```

### Create a new project
```bash
dagster project scaffold --name imagefun-pipeline
```

This will create a project that needs to be opened with `dagster dev` and not `dg dev`


### Definitions and Assets

Definitions act as containers for all Dagster entities in a project.

Definitions contains one or more assets.

Assets are the core building blocks of Dagster: they represent the entities in the data platform. The dependencies between entities define the project's lineage.

An asset can be a database table, ML model, AI process: all the assets form the data platform.

Assets are defined in the `assets.py` file as function with the `@dg.asset` decorator, and needs to be associated with a top-level `Definitions` object in order to be deployed, in the `definitions.py` file.

```
$ src
- dagster_tutorial
-- defs
--- assets.py
-- definitions.py
```

After creating the project and defined assets and definitions, check validity of definitions with `dg check defs` (this ensure that the project is deployable).


#### Materialize the assets
- after checking the definition, go in the Dagster UI (usually at http://localhost:3000) and under the Lineage tab click on Materialize all.


### Resources
Resources are objects like assets, but they are not executed.

Typically resources are reusable objects that supply external context, like database connections, API clients or configuration settings. A single resource can be shared across many different Dagster objects.

This is the case when multiple assets are retrieved from the same provider (like table db or bucket): the resource centralizes the connection in a single object and can be shared across multiple objects.

```
$ src
- dagster_tutorial
-- defs
--- resources.py
```

