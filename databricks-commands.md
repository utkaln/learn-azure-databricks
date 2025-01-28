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
### Spark Read Files
```python
df = spark.read.csv('abfss://<container>@<storage>.dfs.core.windows.net/raw/<filename.csv>')

```
#### Make the first row as header
```python
df = spark.read.option("header",True).csv('abfss://<container>@<storage>.dfs.core.windows.net/raw/<filename.csv>')
```

#### Create a Schema for the incoming data
- In the previous command the data gets created without schema, hence all the data read appears as String type. Defining a schema meets the required format
- StructType is a type to capture the entire row of the data
- StructField is a data type to capture the individual field name in the row
- The last parameter to StructField indicates `IS NULL`
  
```python
from pyspark.sql.types import StructType, StructField, IntegerType, DoubleType, StringType
data_schema = StructType([
  StructField("fieldName1",IntegerType,False),
  StructField("fieldName2",DoubleType,True),
  StructField("fieldName3",StringType,True),
])

data_df = spark.read
  \.option("header",True)
  \.schema(data_schema)
  \.csv('abfss://<container>@<storage>.dfs.core.windows.net/raw/<filename.csv>')

```

#### Select specific columns and add alias to a column name to rename the field (NOT Preferred approach)
```python
selected_cols_df = data_df.select(col("og_col_name").alias("new_col_name"))
```

#### Select specific columns with alias using Spark `DataFrame.withColumnRenamed()`
```python
renamed_col_df = data_df.withColumnRenamed("og_col_name","new_col_name")
```

#### Add a new column with `DataFrame.withColumn()`
- Add timestamp with `current_date`
```python
from pyspark.sql.functions import current_timestamp

updated_col_df = data_df.withColumn("created_timestamp",current_timestamp())
```
#### Add a column with a static string using `lit()` function
```python
from pyspark.sql.functions import lit

updated_col_df = data_df.withColumn("environment",lit("TEST"))
```

#### Write Data to Data Lake in Parquet format
```python
updated_col_df.write.parquet("abfss://container@storage_acct.dfs.windows.core.net/file_path")
```
- The above command will fail if there is already file existing in the path, to overwrite use the following option
```python
updated_col_df.write.mode("overwrite").parquet("abfss://container@storage_acct.dfs.windows.core.net/file_path")
```

