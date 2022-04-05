# dp-203_preparation
This repository contains my personal cheat sheet and anything else I deem useful in preparation for the DP-203 exam.

## Synapse Analysis
	ELT solution
	Open Source Spark
	Preferred for: SQL analyis, reporting, BI, modern data warehousing
	Dedicated vs Serverless SQL Pool
		Serverless SQL Pool = Polybase + Datalake Storage = Query Datalake as if it is a database
		Serverless SQL Pool has no data manipulation features (no insert, update...); need Spark to modify files in data lake
	SQL Pool
		Hash-Distributed tables
			row based
			for high performance
		Replicated tables
			small tables replicated to each compute node
		Round-Robin tables
			fastest for load
			best for staging
	Spark Pool
		Need to use SQL auth to access SQL outside of workspace
		Autoscaling compares Pending vs Free Resources (pending < free for two minutes => scale down; pending > free for one minute => scale up)
	Synapse studio:
		Develop: SQL, Notebooks, Data flows, Power BI
		Data Flow:
			- Mapping Data Flow: for data transformation
			- Wrangling Data Flow: for data preparation
	Polybase:
		Use files on Azure Storage as Datasource as if they where SQL tables
		COPY seems to be better for loading
	Create dedicated loader accounts as the system administrator account is limited to 25% usage of the system
	Manage Workloads:
		Classify: Load and Query; or Ad-Hoc-Query, Dashboard-Query etc.pp... used to assign different resources by classification
		Importance: High importance workloads take precedence over waiting workloads of lower importance
		Isolation: Reserve resources for specific workloads
	Supported file formats: Avro, Parquet, ORC, Delimited Text, Binary, Json, XML, Excel
	Dynamic Management Views: reports on performance, errors, etc; does not report on best practices (e.g. encryption)
## Databricks
	End to end data engineering and data analysis service
	Proprietary, optimized version of Spark
	Preferred for: ML, realtime transformation
	Notebooks have own DBC file extension
	High Concurrency Cluster
		Only supports SQL, Python, and R
	Standard Cluster
		Also supports Scala (Scala cannot be distributed)
	Use Azue Databricks Monitoring Library to log to Log Analytics
	Certifications: HITRUST, AICPA, PCI DSS, ISO 27001, ISO 27018, HIPAA (Covered by MSFT Business Associates Agreement (BAA)), SOC2 (Type 2)
	Big file formats:
		Parquet: column based, binary, compressed, schema append; useful to query only some columns
		AVRO: row based, flexible schema; useful for ETL when all data needs to be read in some rows
	Spark:
		One Driver node, many worker nodes
		Driver and worker nodes use java programs for scheduling
		worker nodes runs executor, that provides slots for tasks
		Supports TEXT, JSON, CSV, JDBC, ORC, PARQUET, DELTA, LIBSVM and HIVE
		Invoke the command spark.streams.active to see the list of active streams
		Catalyst optimizer:
			analyze, optimize logical plan, physical planning, code generation
		Tungsten:
			binary format used by spark to represent data rows
		Concepts:
			Job is a calculation submitted to spark
			Job is broken down into narrow operations as far as possible to create a pipeline
			Pipeline is executed on an executor
			Wide operations mark a stage boundary due to shuffle occuring
			Shuffle means data has to be cached (in tungsten format) and next stage can be executed by another executor
			Spark works backwards in Tasklist in a "depends on" or "already available" matter
	Spark Structured Streaming:
		Event sources: Azure Event Hub, Kafka, TCP/IP...
		specify checkpointlocation to "take-off-where-we-left"
		setStartingPosition to start at beginning or end of stream, sequencenumber or partition
		to read from stream into df: spark.readStream
	Delta Lake:
		Optimized Data Lake storage based on Data Bricks File System (DBFS)
		Can handle very large data
		Provides ACID transaction handling
		1GB is good file size
		Upsert: MERGE INTO my-table USING data-to-upsert
		ZORDER:
			- co-locate data, sorts it and writes it into new parquet files
			- optimizes where-querries
		Time Travel:
			- temporal querries
			- query past versions of records
			- recreate analysis or reports
			- reproduce experiments
			- snapshot isolation for fast changing tables
	Key workspace limits are:
		max number of jobs per hour: 1000
		max number of jobs concurrently running: 150
		max number of notebooks or execution contexts: 150
		max number of azure databricks API calls: 1500
	Data Protection
		Encryption at-rest – Service Managed Keys, User Managed Keys
		Encryption in-transit (Transport Layer Security - TLS)
		File/Folder Level access control lists (ACLs) for Azure Active Directory (AAD) Users, Groups, Service Principals
		ACLs for Clusters, Folders, Notebooks, Tables, Jobs
		Secrets with Azure Key Vault
## Azure Stream Analytics
	Connects to:
		- Azure IoT Hub
		- Azure Event Hub
		- Azure Blob Storage
## Azure Data Explorer
	Data analytics service for real time big data
	Usecases: IoT, Log Streams...
	Uses Data Lake storage and Kusto query language
	Creating external tables in TSV, CSV, JSON and Parquet
	Reading data from many file formats
## Data Factory
	ETL solution
	SDK Support for: .NET, Pyhton, REST, ARM Template, Powershell
	Integration
		Azure SQL Server Integration Services (SSIS)
			capabilities: execute packages on azure
			SSIS package = components like connection manager, tasks, control flow, data flow, parameters, event handlers, variables packaged to perform an ETL task
		Self-hosted
			capabilties: data movement, activity dispatch
			bring your own cluster
			bring your own driver (SAP Hana, MySQL)
		Azure
			capabilities: data flow, data movement, activity dispatch
			no private network!
	Monitoring:
		keeps run logs and data only for 45 days; use Azure Monitor for more days
	Create pipelines with: REST API, Python SDK, .NET SDK, Powershell, Azure CLI, ARM Templates
## Hadoop Distributed File System 
	Name Node: 
		master node
		regulates access to file system
		instructs data nodes
	Data Node:
		worker node
		performs file system operations
## HDInsight
	Spark as a service
	Autoscaling
	In-memory caching of data
	Query from external relational stores
	Support for firewall
	Supports: Spark, Hive/Interactive Query, HBASE, Kafka, Storm, Hadoop, Ooozie, ZooKeeper, Ambari
## Azure SQL
	Need to configure Azure AD Administrator if AD users should be able to connect to database
	Encryption
		Deterministic: supports joins and equality operations etc., guessable
		Randomized: does not support joins or equality, more secure
	Security
		Row Based: restrict access to rows with a predicate function
		Column based: restrict columns when granting access
		Data masking
			obscure data in result sets, e.g. aXXX@XXXXA.com for emails
			does not change data in database
	Firewall
		Server and Database level possible
		128 rules possible
		Server rules:
			can be defined with portal, powershell or T-SQL
		Database rules:
			"override" server rules (if database rule is defined with range A and server rule with disjunct ip range B, only IPs in range A can access the databse)
			can be defined with T-SQL
			can be defined only if a server rule is present
	Elastic pools:
		Used for multi tennant applications
		Single-tenancy: Each database stores data from only one tenant.
		Multi-tenancy: Each database stores data from multiple separate tenants (with mechanisms to protect data privacy).
		Hybrid also available
	Data Sync
		Sync data between on-prem and cloud
	Isolation Levels: ReadCommitted, ReadUncommitted, RepeatableRead, Serializable, Snapshot
## Data Migration Assistant
	Analyze Migration
	Detect compatibility issues with SQL Server or Azure SQL
## Cosmos DB
	Speed up query with lookup tables
	Achieve good partitioning with synthetic partition keys
	Can configure default TTL and override it on item level
	Synapse Link:
		HTAP solution
		real time analysis on data in Cosmos DB
		two types of schema: well defined and full fidelity
	With Azure Synapse Link:
		- Can define default TTL for analytical store independently from transactional store
		- Cannot override TTL on item level
		- Cannot disable azure synapse link, you have to delete the cosmos db account
		- Can have non-analytic container in cosmos db account with synapse link
	Available APIs: MongoDB, Cassandra, Gremlin, SQL, Table
	Consistency Levels:
		Strong - strongest
		Bounded staleness	
		Session
		Consistent prefix
		Eventual - weakest
## Azure Storage
	250 storage accounts per subscription with 500 TB each
	Azure Tables:
		- like Cosmos DB a NoSQL Database
	Azure Data Lake:
		- use passthrough to let configured clusters access data
	Stores logs in $logs if Storage Analytics is enabled
	Use Azure Files or Azure Disks as persistent volumes for containers
## Data Lake Analytics
	big data analysis solution	
	U-SQL, R, Python, and .NET
## T-SQL
	WHERE => no data movement between compute nodes
	JOIN => might have data movement between compute nodes
## Transactional Databases
	ACID
		Atomic: All or nothing
		Consistent: Data is consistent before and after transaction
		Isolation: Transaction has no effect on other transactions
		Durability: Once transaction is deemed complete, the system has persisted the new data
	OLAP (Online Analytics Processing)
		long running and complex querries by few users
	OLTP (Online Transaction Processing)
		short transactions by many users
	HTAP (Hybrid Transactional and Analytical Processing)
		hybrid of OLAP and OLTP; azure synapise link is HTAP
## Patterns
	Star Pattern
		Fact Table that is rapidly changing (Hash-Distribution); contains references to dimensions
		Dimension Table that contain not-so-often-changed values of entities (like email, name); (Replication)
		Staging Table contains raw data for loading; (Round Robin)
	Snowflake Pattern
		Like star pattern, but dimensions are normalized
	Slowly Changing Dimensions
		value of an entity sometimes changes (like user email)
		SCD1: Use "audit"-columns to know when dimension was changed.
		SCD2: Use new row to have historic data.
		SCD3: Use new column (instead of new row) to store old value (e.g. CurrentEmail, OldEmail)
		SCD6: Combination of 1,2 and 3. Would store historic data as another column and new data in a new row. So "unchanged" fields can be tracked aswell.
	Data Loading
		Load into datalake as staging area to:
			- load from different sources
			- reduce contention on source system
			- rerun failed loads
		Keep zones in datalake for data of various degrees of "cleanliness"; like bronze for raw, silver for refined and gold for "ready for analytics"
		Use compression to minimize transfer times and make use of parallel processing
		When splitting input files, use multiples of your compute nodes so all nodes are used
	Modern Data Warehouse:
		Stages: Ingest, Store, Prep&Train, Model&Serve
	Flattening nested schemas
		Define function for flattening
		Flatten nested schema
		Explode arrays
		Flatten child nested schema
Data Share
	Share data to other subscriptions
	Copies data
	Workflow:
		1. Create Data Share
		2. Add Datasets (SQL Database, Datalakes) to Data Share
		3. Invite users
		4. As invited user, accept invite
		5. Map datasets in datashare to own resources
		6. Trigger data refresh. Then everythin will be copied to own resources.
Temporal querrying:
	Using Windows
	Tumbling:	Fixed size, non-overlapping, fixed interval
	Hopping: Fixed size, overlapping, fixed interval
	Sliding: Fixed size, overlapping, flexible interval (only when events enter or leave window)
	Session: Flexible size, non-overlapping, flexible interval (only when events occur, window extends with occurrence before timeout)
## Process
	Agile focuses on processes highlighting change while accelerating delivery.
	CI/CD focuses on software-defined life cycles highlighting tools that emphasize automation.
	DevOps focuses on culture highlighting roles that emphasize responsiveness.
# Abbreviations:
| Abbreviation | Meaning |
| ------------- | ------------- |
| CETAS | create external table as select |
| CTAS | create table as select |
| T-SQL | transact SQL |
| TSV | tab separated |
| CSV | comma separated | 
| SU | Streaming Units | 
| ITSM | IT Service Management | 
| HTAP | Hybrid Transactional and Analytical Processing | 
| TDE | Transparent Data Encryption | 
# Further reading
https://docs.microsoft.com/en-us/azure/databricks/scenarios/store-secrets-azure-key-vault
https://docs.microsoft.com/en-us/azure/synapse-analytics/sql/overview-features
https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-twitter-sentiment-analysis-trends
https://docs.microsoft.com/en-us/azure/cosmos-db/synthetic-partition-keys
https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-performance
