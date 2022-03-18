# dp-203_preparation
This repository contains my personal cheat sheet and anything else I deem useful in preparation for the DP-203 exam.

## T-SQL

	WHERE => no data movement between compute nodes
	JOIN => might have data movement between compute nodes

## Synapse Analysis
	ELT solution
	Open Source Spark
	Preferred for: SQL analyis, reporting, BI, modern data warehousing
	SQL Pool
		Hash-Distributed tables
			row based
			for high performance
		Replicated tables
			small tables replicated to each compute node
		Round-Robin tables
			fastest for load
			best for staging
	Synapse Link:
		real time analysis on data in Cosmos DB
		two types of schema: well defined and full fidelity
	Synapse studio:
		Develop: SQL, Notebooks, Data flows, Power BI
## Databricks
	End to end data engineering and data analysis service
	Proprietary, optimized version of Spark
	Preferred for: ML, realtime transformation
	High Concurrency Cluster
		Only supports SQL, Python, and R
	Standard Cluster
		Also supports Scala (Scala cannot be distributed)
	Use Azue Databricks Monitoring Library to log to Log Analytics
	Upsert: MERGE INTO my-table USING data-to-upsert
	Certifications: HITRUST, AICPA, PCI DSS, ISO 27001, ISO 27018, HIPAA (Covered by MSFT Business Associates Agreement (BAA)), SOC2 (Type 2)
	Big file formats:
		Parquet: column based, binary, compressed, schema append; useful to query only some columns
		AVRO: row based, flexible schema; useful for ETL when all data needs to be read in some rows
## HDInsight
	Spark as a service
## Data Factory
	ETL solution
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
			no private network
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
## Azure SQL
	Encryption
		Deterministic: supports joins and equality operations etc., guessable
		Randomized: does not support joins or equality, more secure
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

# Abbreviations:
| Abbreviation | Meaning |
| ------------- | ------------- |
| CETAS | create external table as select |
| CTAS | create table as select |
| T-SQL | transact SQL |
| TSV | tab separated |
| CSV | comma separated | 

# Further reading
https://docs.microsoft.com/en-us/azure/databricks/scenarios/store-secrets-azure-key-vault
https://docs.microsoft.com/en-us/azure/synapse-analytics/sql/overview-features