# learn-azure-databricks

## Core Concepts
### Spark
- At the core is RDD ( Resilient Distributed Datasets): a collection of items that can run in parallel in distributed systems
- Direct APIs are hard to code 
- Spark SQL Engine - Simplifies interaction with Spark easier
- Spark SQL - Collection of APIs for :
    - Streaming
    - ML
    - Graph
- Spark also comes with a standalone resource manager similar to YARN or Kubernetes

### Databricks
- Databricks provides ability to create Spark cluster easily with libraries for specific needs
- Provides Jupyter notebook for easier development
- Provides Admin Controls
- Optimized Spark runtime (5x faster)
- Ability create Database and Tables
- Delta Lake (Open Source) - Provides ACID capabilities
- SQL Analytics
- ML Flow

### Azure
- Can directly buy data bricks from azure instead of installing on a VM
- Unified portal, billing, security, single sign using Active Directory
- Integration with Power BI, Azure ML, Azure DevOps, DB
- Ingestion of data using Azure Data Factory

## Databricks Cluster Configuration
- Control Plane
    - Databricks cluster manager
    - Databricks UX
    - DBFS (not for actual data to be processed)
- Data Plane
    - Vnet
    -  Network Security Group (NSG)
    - Azure Blob Storage
    - Database Workspace
- Two types of clusters : All Purpose (Manual) | Job Clusters
    - **All Purpose cluster** : 
        - Created via the UI, CLI or API manually 
        - Can be reused and shared
        - Persistent and Expensive
    - **Job Cluster**:
        - Created by batch programs for automatic ETL or ML jobs. This is usually started from Workflows
        - Can’t be retained or shared. Starts and terminates with the required job purpose
        - Less expensive
- **Mode**:
    - Single Node : One node acting as driver and worker node, does not scale up horizontally 
    - Multi Node : Dedicated node for driver and worker nodes. Scales up as required
- **Access Mode**: 
    - Single User : One user access
    - Shared : multiple users can access but individual tasks are not shared, so one failure lets other users continue
    - No Isolation Shared : same as shared but process nodes are shared, so one failure blocks everyone
 
## How To
### Create Cluster and Create a Notebook
  - Creating standalone cluster takes about 5 minutes
  - Creating a pool earlier saves time in cluster start up
  - Workspace can be created if a cluster is running
  - Create a Workspace then create Notebook
 
### Magic Commands:
  - Use these commands to dynamically choose language and syntax in the notebook such as switching to SQL, FileSystems, Scala etc. while running in Python mode
  - Example: `%fs ls` to list files from the file system. `%scala print(msg)` is to compile a command using Scala language
  - Helps with adhoc queries 
 
### Databricks Utilities  
  - `dbutils.fs.ls('/some_dir')` - List files under some_dir
  - Useful with programmatic use of different methods


## Azure Data Lake
### Access to Data Lake Storage
1. Using Access Keys (Session Scoped)
2. Using SAS Tokens (Session Scoped)
3. Using Service Principal (Session Scoped)
4. Using Cluster Scoped Auth
5. Using Unity Catalog

### How to Create Azure Data Lake Storage
1. Follow the steps to Create **Azure Storage Account First** : Create Resource > Storage Account > Create
2. Choose: Pay As You Go , Choose resource group for the project, name the storage account name unique, Local Redundancy is fine for tutorial, Performance : Standard
3. Ensure choosing Enable Hierarchical namespace. This helps in organizing storage for Data Lake
4. Leave deafult option for other tabs
5. Create folders to contain the data. This is known as the name `Container`. Create Three folders : `raw`, `processed` and `presentation`
   
