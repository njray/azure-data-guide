# Data Pipeline

**In this article**

[About]()  
[Extract, Transform, and Load](#etl)  
[Extract, Load, and Transform](#elt)  
[Data Flow & Control Flow](#dataflowcontrolflow)  
[Where to go from here](#wheretogo)  

<a name="about"></a>

Gathering data from multiple sources, in multiple formats, and moving it to one or more data stores is a common problem organizations face on a daily basis. The destination may not be the same type of data store as the source, and oftentimes the format is different, or the data needs to be shaped or cleaned in some fashion prior to loading it into its final destination.

Various tools, services, and processes have been developed over the years to help address these challenges. No matter the process used, there is a common need to coordinate the work and apply some level of data transformation within the data pipeline. The following highlight the common methods used to perform these tasks.

## <a name="etl"></a> Extract, Transform, and Load

Extract, Transform, and Load (ETL) is a data pipeline used to collect data from various sources (that's the extract step), transform the data according to business rules, and load it into a destination data store. The transformation work in ETL takes place in a specialized engine, and oftentimes involves using staging tables to temporarily hold data as it is being transformed and ultimately loaded to its destination.

The data transformation that takes place usually involves various operations, such as filtering, sorting, aggregating, joining data, cleaning data, de-duplicating and validating data.

![Extract-Transform-Load (ETL) process](./images/etl.png)

Oftentimes, the three ETL phases are run in parallel to save time. For example, while data is being extracted, a transformation process could be working on data already received and prepare it for loading, and a loading process can begin working on the prepared data, rather than waiting for the entire extraction process to complete.

Relevant Azure service:
- [Azure Data Factory v2](https://azure.microsoft.com/services/data-factory/)

Other tools:
- [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services)

## <a name="elt"></a> Extract, Load, and Transform

Extract, Load, and Transform (ELT) pipeline differs from ETL solely in where the transformation takes place. In the ELT pipeline, the transformation occurs in the target data store. The processing capabilities of the target data store are used to implement the transformation (typically authored in SQL), as opposed to the extra set of technologies the transformation engine may introduce. This simplifies the data pipeline somewhat, by removing the transformation engine from the pipeline, but only works well when the target system is powerful enough to efficiently transform the data. The side benefit to this approach is that scaling the target data store also scales the ELT pipeline performance.

![Extract-Load-Transform (ELT) process](./images/elt.png)

Typical use cases for ELT fall within the big data realm. As an example, you might start by extracting all of the source data to flat files in scalable storage such as HDFS or Azure Data Lake Store. This enables the loading of data from various sources so it can be queried with technologies that can query these scalable stores such as Spark, Hive or PolyBase. The key point with ELT is that the data store used to perform the transformation is the same data store from which the data may ultimately be consumed and this data store reads the data directly from the scalable storage instead of loading the data into proprietary storage it manages. This approach skips the data copy step present in ETL, which for large data sets can be a time consuming operation. 

In practice the data store is a data warehouse using either a Hadoop cluster (using Hive or Spark) or a SQL Data Warehouse. In general, a schema is overlayed atop the flat file data at query time and stored as a table, enabling the data to be queried like any other table in the data store. These are referred to as external tables because the data does not reside in storage managed by the data store itself, but on some external scalable storage. The data store, for its part, only manages the schema of the data and applies the schema on read. For example, a Hadoop cluster using Hive would describe a Hive table where the data source is effectively a path to a set of files in HDFS. In SQL Data Warehouse, Polybase is used to achieve the same result- creating a table against data stored external to the database itself. Once the data is loaded in this way, the data present in the external tables can be processed using the capabilities of the data store. In big data scenarios, this means the data store must be capable of massively parallel processing (MPP) which breaks the big data down into smaller chunks and distributes processing of the chunks across multiple machines in parallel. 

The final phase of the ELT pipeline is typically to transform the source data into a final format that is more performant with respect to the types of queries that will need to be supported. Examples of this include partitioning the data and using optimized storage formats like Parquet that store row oriented data in a columnar fashion and provides indexing that optimizes reading from only those files that contain the data requested by the query. 

Relevant Azure service:
- [Azure SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)
- [HDInsight with Hive](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-use-hive)
- [Azure Data Factory v2](https://azure.microsoft.com/services/data-factory/)
- [Oozie on HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-oozie-linux-mac)

Other tools:
- [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services)

### <a name="dataflowcontrolflow"></a> Data Flow & Control Flow

In the context of data pipelines, the control flow ensures orderly processing of a set of tasks. To enforce the correct processing order of these tasks, precedence constraints are used. You can think of these constraints as connectors in a workflow diagram, as shown in the image below. Each task has an outcome, such as success, failure, or completion. Thanks in part to the precedence constraints, any subsequent task does not initiate processing until its predecessor has completed with one of these outcomes.

Data flows differ from control flows, in that tasks, or components within a data flow can run in parallel with one another. These are typically source, transformation, and destination task items. Data flows are executed by control flows as a task. They are responsible for transforming data, and the data is transferred from one data flow task to another. Unlike control flows, you cannot add constraints between tasks in a data flow. You can, however, add a data viewer to observe the data as it is processed by each task.

![Data Flow being executed as a task within a Control Flow](./images/control-flow-data-flow.png)

In the diagram above, there are several tasks within the control flow, one of which is a data flow task. One of the tasks is nested within a container. Containers can be used to provide structure to tasks, providing a unit of work. One such example is for repeating elements within a collection, such as files in a folder or database statements.

Relevant Azure service:
- [Azure Data Factory v2](https://azure.microsoft.com/services/data-factory/)

Other tools:
- [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services)

## <a name="wheretogo"></a>Where to go from here

Read Next: [Data Warehousing Solution Pattern](../pipeline-patterns/data-warehousing.md)

See Also:

Related Pipeline Patterns
- Working with transactional data
    - [Online Transaction Processing (OLTP)](../pipeline-patterns/online-transaction-processing.md)
    - [Online Analytical Processing (OLAP)](../pipeline-patterns/online-analytical-processing.md)
    - [Data Warehousing](../pipeline-patterns/data-warehousing.md)

Related Technology Choices
- Transactional Data Stores
    - [Online Transaction Processing (OLTP) data stores](../technology-choices/oltp-data-stores.md)
    - [Online Analytical Processing (OLAP) data stores](../technology-choices/olap-data-stores.md)
    - [Data Warehouses](../technology-choices/data-warehouses.md)
- [Pipeline Orchestration, Control Flow, and Data Movement](../technology-choices/pipeline-orchestration-data-movement.md)
