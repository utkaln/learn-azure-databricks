# Basics I/O
## SQL Commands 
### Unity Catalog Related Commands
- Unity catalog maintains data under three hierarchies :
  - Catalog
    - Schema
      - Table  
```SQL
SHOW CATALOGS
SHOW SCHEMAS
SHOW TABLES

SELECT current_catalog()
SELECT current_schema()

-- To use the command to just refer by table name, first set the following to environments
USE CATALOG <catalog_name>
USE SCHEMA <schema_name>

```

## Python Commands
```bash
# Show tables
%python
display(spark.sql('SHOW tables'))

#  view data from table with a fully qualified name
%python
display(spark.table('<catalog>.<schema>.<table>'))
```

# Data Ingestion
#### Spark Read Files
```spark
df = spark.read.csv('abfss://<container>@<storage>.dfs.core.windows.net/raw/<filename.csv>')
```
- Make the first row as header
```spark
df = spark.read.option("header",True).csv('abfss://<container>@<storage>.dfs.core.windows.net/raw/<filename.csv>')
```
