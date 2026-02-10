#models #model #sql #dbt #dbtmodel
- are just SQL select statements used to modify data
- can have Python models in DBT --> check out Python videos
- Layer of logic between raw and transformed layer
- dbt abstracts away data manipulation language and data deifnition language to make it easy to choose between views and tables

**Table:** a structured set of data held in a database, consisting of rows and columns. Each column represents a different attribute of the data, and each row corresponds to a single record. Tables are fundamental components in databases and are used to store and organize data efficiently.

**View**: a database object that stores a SQL query rather than the data itself. When you query a view, the database runs the stored SQL query and returns the results.

Tables or views

dbt will by default materialise sql queries as views in the data platform when running the command `dbt run` on a file in the models folder of the git repo.

However, we can add this line at the top of our file if we want to materialise the data object as a table instead

```
{{ config(materialized='table') }}
```

Also, in the global configuration file `dbt_project.yaml` you can specify materialisation, and set it at a folder level to avoid need to include above line in each model.

```
models:
  my_new_project:
    # Applies to all files under models/example/
    example:
      +materialized: table
```

Materialisation set at the model level, will override that set at the global level, so, model specific materialisation is used as an exception to the global configuration when needed.

The curly brace syntax of the materialisation command is a template language called #jinja and is not SQL. The config command is a macro built into the dbt project.

The lineage view shows the directed acyclic graph (DAG)

![[Pasted image 20260210104840.png]]

You can query the whole model using the name in the dbt_project.yaml

You can also query a specific node, and use the following syntax to express how many steps up and/or downstream you want to see:

```
2+customers+2
```

## Modularity

Instead of building one perfect SQL query for the final model, we can use modularity and assemble the final view from pieces, that could be reused in other models.

### Staging tables

So, to modularise our models/queries, we may take out some of the CTEs to be their own model. These are 1:1 with source tables, i.e no joining, and should be named with the following convention; "stg" short for staging table, underscore, the schema name, then double underscore and the table name.

```
stg_schema_name__table_name.sql
```

example

```
stg_jaffle_shop__customers.sql
```


When referencing a staging table or other model which is not source, we need to use dynamic referencing, because dbt will use our personal development schema, and this wont run in production, or for other users.

```
with customers as (
    select * 
	from analytics.dbt_tball.jaffle_shop.stg_jaffle_shop__customers.sql
)
```

for this we need to use the `ref` macro, and we reference the model name as a string argument in single quotes, without the `.sql` file extension

```
{{ ref('stg_jaffle_shop__customers') }}
```

To use autocomplete with macros, type double underscore first

