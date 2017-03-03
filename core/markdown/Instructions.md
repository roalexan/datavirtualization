## Resource provisioning (manual)

Okay, almost done! You need to do these two steps before you can test the solution:

1. Login to the VM whose IP address is **{Outputs.virtualMachineIP}** using the user name and password you specified during deployment. Typically this is done via RDP, but feel free to use your own preferred client. If you do use RDP, you can either open up RDP and enter the IP address, or double click the .RDP file. You can download the .RDP file by clicking [here]({Outputs.virtualMachinePortalUri}) and then clicking **Connect**.
1. From the VM, double click the shortcut **Setup - Data Virtualization** on the desktop and wait for the script to finish. It typically takes about 10-12 minutes to run.

> **At this point you will need to wait until the setup on your VM finishes before you proceed to the next section. Also, keep your session to your VM open, as you will use it in the next section. **

## Take it for a spin

Congratulations! You have successfully deployed the data virtualization tutorial.

Now that everything is deployed, you're ready to explore two data virtualization techniques:
- Query scale out
- Hybrid execution

## Query scale out

Query scale out shows how to scale a query from a hardware constrained SQL Server 2016 to an HDInsight cluster using PolyBase. First, let's test a query running entirely in the SQL Server.

### Run on local resources only

So, first we're going to look at the case where everything runs in your SQL Server. You will be running a query that joins table data entirely on the SQL Server running on your virtual machine. Note that for the purposes of this tutorial we allow the sample query to run on the SQL server without adding any extra load or other query executed in parallel. You can imagine, however, that in an actual production environment there would be hardware constraints - memory, CPU, I/O - that would adversely affect performance. It is also very possible that the sheer amount of data would not fit in your SQL Server.

Using your existing client session to your virtual machine, do the following steps:

1. Double click **SQL - Data Virtualization** on your desktop to open the sample SQL folder
1. Double click **select-without-pushdown.sql**
1. Double click **SSMS** (SQL Server Management Studio). It takes a few minutes for SSMS to come up the first time
1. Click **Connect** using all default values
1. Click anywhere on the SQL statement to get focus then click **Execute**. You can also click Function F5 if you want. Note the execution time. This will vary, but typically will exceed one minute. As of this writing, it took 1 minute and 16 seconds.

### Run by scaling out to Hadoop (distributed environment)

Now let's look at how you can speed things up by offloading some of the work to HDI. We're going to run the same query, but this time the where clause will be executed on HDI. The where clause is sent to HDI, where map reduce jobs are run to crunch the data and return only the results, where it is incorporated back into the local query execution. Most production Analytics systems already have data archived in big data stores that can come in handy when implementing this technique for scaling out SQL query execution without the need to move data. This approach can be used to significantly speed up your query execution times.

Again using your client session to your virtual machine, do the following steps:

1. Double click **SQL - Data Virtualization** on your desktop to open the sample SQL folder
1. Double click **select-with-pushdown.sql**
1. Click anywhere on the SQL to get focus then click **Execute**. You can also click Function F5 if you want. Note the execution time. This will vary, but typically will be less that one minute. As of this writing, it took 55 seconds.

## Hybrid execution

Hybrid execution shows how to read and join table data stored your SQL Data Warehouse with table data stored in an Azure blob. We will be running Scala code in Jupyter Notebook running on your HDI cluster. As you can imagine, many production systems need reference data stored in SQL databases/datawarehouses (such as sales or location data) to transform (ETL) the data residing in big data stores like Azure storage, Azure Data Lake, etc. used by HDI.

Using a browser of your choice, do the following steps:

1. Browse to the Jupyter Notebook installed on your HDI cluster by clicking [here]({Outputs.jupyterNotebookUri})
1. Expand the **Scala** folder
1. Click the **Data Virtualization.ipynb** IPython notebook
1. Login using the same user name and password you used during the deployment
2. Select and run each of the cells from top to bottom, waiting for each cell to finish before moving on to the next one. You can tell when a cell has finished executing because the * will no longer be there.

## Cleaning Up

> **When you are all done, you should delete this solution or you will continue to be charged for hosting these resources.**