# Options for Pipeline Orchestration, Control Flow, and Data Movement

[About]()  
[What are your options for pipeline orchestration, control flow, and data movement?](#options)  
[How do you choose?](#howtochoose)  
[Key Selection Criteria](#criteria)  
[Capability Matrix](#matrix)   
[Where to go from here](#wheretogo)  

<a name="about"></a>

## <a name="options"></a> What are your options for pipeline orchestration, control flow, and data movement?
In Azure, the following services and tools will meet the core requirements for pipeline orchestration, control flow, and data movement:

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/)
- [Oozie on HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-oozie-linux-mac)
- [SQL Server Integration Services](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services)

## <a name="howtochoose"></a> How do you choose?
These services and tools can be used independently from one another, or at times, used together to create a hybrid solution for your data pipeline needs. For instance, the new Integration Runtime (IR) in Azure Data Factory V2 is capable of natively executing SSIS packages in a managed Azure compute environment. While there is some overlap in functionality between these services, there are a few key differences that will lead you to select one over the other in certain situations.

## <a name="criteria"></a> Key Selection Criteria

Answer the following questions to help you narrow down your choices, then use the matrices below to select your options for your scenario:

- Do you need big data capabilities for moving and transforming your data? Usually this means multi-gigabytes to terabytes of data.
    - If yes, then narrow your options to those that best suited for big data.
- Do you require a managed service that can operate at scale?
    - If so, select one of the cloud-based services that aren't limited by your local processing power.
- Are some of your data sources located on-premises?
    - If yes, look for options that can work with both cloud and on-premises data sources or destinations.
- Is your source data stored in blob storage on an HDFS filesystem?
    - Look for options that support Hive queries.

## <a name="matrix"></a> Capability Matrix

### General Capabilities

| | Azure Data Factory (ADF) | SQL Server Integration Services (SSIS) | Oozie on HDInsight
| --- | --- | --- | --- |
| Managed | Yes | No | Yes |
| Cloud-based | Yes | No (local) | Yes |
| Prerequisite | Azure Subscription | SQL Server  | Azure Subscription, HDInsight cluster |
| Management Tools | Azure Portal, PowerShell, CLI, .NET SDK | SSMS, PowerShell | Bash shell, Oozie REST API, Oozie web UI |
| Pricing | Pay per usage | Licensing / Pay for features | No additional charge on top of running the HDInsight cluster |

### Pipeline Capabilities

| | Azure Data Factory (ADF) | SQL Server Integration Services (SSIS) | Oozie on HDInsight
| --- | --- | --- | --- |
| Copy Data | Yes | Yes | Yes |
| Custom Transformations (C#) | Yes | Yes | Yes (MapReduce, Pig, and Hive jobs) |
| Azure ML Scoring | Yes | Yes (with scripting) | No |
| HDInsight On-Demand | Yes | No | No |
| Azure Batch | Yes | No | No |
| Pig, Hive, MapReduce | Yes | No | Yes |
| Spark | Yes | No | No |
| Execute SSIS Package | Yes | Yes | No |
| Control Flow | Yes | Yes | Yes |
| Access On-Premises Data | Yes | Yes | No |

### Scalability Capabilities

| | Azure Data Factory (ADF) | SQL Server Integration Services (SSIS) | Oozie on HDInsight
| --- | --- | --- | --- |
| Scale Up | Yes | No | No |
| Scale Out | Yes | No | Yes (by adding worker nodes to cluster) |
| Optimized for Big Data | Yes | No | Yes |

## <a name="wheretogo"></a>Where to go from here
Read Next:
[Data Warehouse Solution Pattern](../pipeline-patterns/data-warehousing.md)

See Also:

Related Pipeline Patterns
- Working with transactional data
    - [Online Transaction Processing (OLTP)](../pipeline-patterns/online-transaction-processing.md)
    - [Online Analytical Processing (OLAP)](../pipeline-patterns/online-analytical-processing.md)
    - [Data Warehousing](../pipeline-patterns/data-warehousing.md)

Related Technology Choices
- Transactional data stores
    - [Online Analytical Processing (OLAP) data stores](../technology-choices/olap-data-stores.md)
    - [Data Warehouses](../technology-choices/data-warehouses.md)
