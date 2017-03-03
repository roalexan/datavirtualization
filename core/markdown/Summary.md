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
We want to build a system where an ad-hoc SQL query lands on SQL Server 2016 where part of the SQL is sent to a Hadoop cluster for scalable execution. For the relational database we will be deploying SQL Server 2016 on IAAS (on a VM) because we will need a customized VM containing SQL Server 2016, PolyBase, and the AdventureWorks database - the VM is pre-built and made available on Azure blob storage. PolyBase (https://msdn.microsoft.com/en-us/library/mt143171.aspx) is a technology that accesses and combines both non-relational and relational data, all from within SQL Server. It allows you to run queries on external data in Hadoop or Azure blob storage. The queries are optimized to push computation to Hadoop. For the Hadoop cluster we will be deploying an HDInsight Spark cluster.

The architecture for this use case can be illustrated as:

<img src="{PatternAssetBaseUrl}/queryscaleoutazure.png" width="100%">

#### Hybrid Execution
We want to build a system where a query lands on a Hadoop cluster where referential data is pulled in from a relational database and joined with data stored in Azure blob. For the Hadoop cluster we will use the same HDInsight Spark cluster. For the relational database we be deploying Azure SQL DataWarehouse with AdventureWorks.

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

#### Hybrid Execution

## Pricing
Your Azure subscription used for the deployment will incur consumption charges on the services used in this solution.
>**Please delete the solution when you are doing using it.**