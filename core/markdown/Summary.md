## Summary

Demonstrates data virtualization techniques on the Azure stack. The first technique shows how to scale a query from SQL Server 2016 to an HDInsight cluster using PolyBase. The second technique shows how to run a query from an Azure HDInsight cluster against an Azure SQL DataWarehouse using JDBC.

## Description

Data Virtualization, What Is It Good For? Absolutely everything! Well, no, but there are some very real benefits from using data virtualization. Here are a few:
- Offload large computation to more powerful processing systems and seamlessly merge the results.
- Combine cloud scale compute with on-premises environments.
- Access large collections of disparate data sources across system boundaries (example: separate systems for marketing and sales data).
- Enable queries against large datasets without the need to move then to a single system (potentially with limited storage).
- Reduce heavy network I/O by moving the compute to the data.
- Simplify access to all sorts of data using a single query language (example: SQL).
- Avoid the need to replicate business logic across multiple systems.

So, how does one design systems that enable data virtualization? To answer this question, we will explore two data virtualization techniques: query scale-out and hybrid execution.

#### Query Scale-Out
Say you have a multi tenant SQL Server running on a hardware constrained environment. How can you offload some of the work to speed up the queries? How can you access vast amount of data that won't fit in the SQL Server? PolyBase to the rescue!

PolyBase (https://msdn.microsoft.com/en-us/library/mt143171.aspx) is a technology that accesses and combines both non-relational and relational data, all from within SQL Server. It allows you to run queries on external data in Hadoop or Azure blob storage. The queries are optimized to push computation to Hadoop.

We will deploy a solution where a SQL query lands on SQL Server 2016 where part of the SQL is sent to a Hadoop cluster for scalable execution using PolyBase. For the relational database we will be deploying SQL Server 2016 on IAAS (on a VM), where we will also install PolyBase and the AdventureWorks sample OLTP database. For the Hadoop cluster we will be deploying an HDInsight Spark cluster.

The architecture for this use case can be illustrated as:

<img src="{PatternAssetBaseUrl}/queryscaleoutazure.png" width="100%">

#### Hybrid Execution
Say you have ETL processes that run over your unstructured data and store it in blob. How can you join this blob data with referential data stored in a relational database?

We will deploy a solution where you will see how to join your data in Azure blob storage with your data in Azure SQL Data Warehouse. For the Hadoop cluster we will use the same HDInsight Spark cluster. For the relational database we will deploy Azure SQL DataWarehouse, where we will also install the AdventureWorks sample data warehouse.

The architecture for this use case can be illustrated as:

<img src="{PatternAssetBaseUrl}/hybridexecutionazure.png" width="100%">

## Deployment
Click **Deploy** to deploy this solution. The automatic deployment step takes about **25 minutes**. There is also a manual deploy step that takes about **10 minutes**. The following resources will be deployed to your Azure subscription automatically:

- HDInsight cluster
- Virtual machine
- Public IP address
- Network interface
- Network security group
- Virtual Network
- Storage account
- Load balancer
- SQL Server
- SQL data Warehouse

As part of the deployment, the virtual machine will invoke a custom script extension to:
- Install the JRE
- Install PolyBase
- Create AdventureWorks2012 on the local SQL Server 2016
- Populate AdventureWorksDW2012 on the Azure SQL DataWarehouse

## Running
After the solution is deployed, you will explore two data virtualization techniques: query scale-out and hybrid execution.

#### Query Scale-Out
For this data virtualization technique you will see how you can speed up your query execution by sending parts of your SQL statement for remote execution. For this tutorial, you will deploy a virtual machine running SQL Server 2016 loaded with the AdventureWorks database. It will also have custom table, BigProduct, containing millions of rows. You will first do a query where all the processing happens in SQL Server. Because all processing happens locally, the execution time is slower than when run on a larger distributed system. Also, in the real world, you may be running in a hardware constrained environment competing with many other queries.

~~~~
...
FROM
  Production.BigProduct p
...
WHERE
  p.ProductID > 50
ORDER BY
  p.ProductID;
  ~~~~

Next, you will run the same query but this time using an external table, BigProduct_HDFS, and the directive to force external pushdown. The external table contains the same data in an HDInsight cluster. This syntax tells PolyBase to run the where clause of your SQL statement on the remote HDI cluster and only return the results for local processing. This allows you to take advantage of the parallel computation capabilities of a large Hadoop cluster, potentially speeding up query execution.

~~~~
...
FROM
  Production.BigProduct_HDFS p
...
WHERE
  p.ProductID > 50
OPTION (
  FORCE EXTERNALPUSHDOWN
);
~~~~

#### Hybrid Execution

For this data virtualization technique you will see how you can run a query from an HDI environment that joins data in an Azure SQL DataWarehouse with data in an Azure storage blob. In essence you will see how to join unstructured data stored in blob with structured data stored in a relational system. This comes into play when you consider that you might have a series of ETL that does some processing on the unstructured data and stores it to blob, which then needs to be joined with referential data stored in a relational system.

After everything is deployed, you will be provided a link that takes you directly to a Jupyter notebook, running on your HDI cluster, where you will run Scala code showing how to join the data on the two systems.

You will first setup the Spark environment
<img src="{PatternAssetBaseUrl}/setupspark.png" width="100%">

Next create the JDBC connection to the table in Azure SQL Data Warehouse
<img src="{PatternAssetBaseUrl}/definesqltable.png" width="100%">

Next create the connection to the table data in Azure store blob
<img src="{PatternAssetBaseUrl}/defineblobtable.png" width="100%">

And finally join the table data
<img src="{PatternAssetBaseUrl}/jointables.png" width="100%">

## Cleanup
When you are done with this solution, please remove it so you don't continue to incur charges. You can do this selecting your deployed solution and clicking **Clean up Deployments**.