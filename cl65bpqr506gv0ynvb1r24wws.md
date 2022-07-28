## Incrementally loading data using Databricks Autoloader

# Background

One of the most common use cases we have had in the ETL workloads is tracking which incoming files have been processed and incremental processing of the new data coming in from the sources. During early days of career, I still remember we had this File Watcher component in our framework which used to keep watching for the files in the landing folder. The file watcher used to register each file coming into the landing layer and once all the required conditions were met the component used to process all the data into the DataWarehouse.

Even though a lot has changed in last 15 years in terms of technology and we have moved from strictly on-prem to "Let's build Data Platform on Cloud" strategy, few basic concepts are still the same and form the core of any data platform. Even till date we have the same problem statement where we receive the data from the source systems which we would like to process incrementally and also achieve the guarantee of processed exactly one for every file.  

This is a multi-part blog and I will be covering AutoLoader, Delta Live Tables and Workflows in this series. This is the first part of the series where I cover Databricks AutoLoader. 

# Ingesting Data from Source - The Traditional Way

Typically, we all would like to have an Event driven orchestration and below is what you would like to do: 

1.  Source data is extracted by a Data Extraction pipeline and dumped into a landing layer. You would expect the pipeline to extract data incrementally every time there is a change at the source. So you would expect a file per day or many times a day. May be a file with all changes every couple of minutes or even a minute. 
2.  The moment the file lands into the landing layer, which can be typically a object based storage like an Azure blob or Azure Data Lake Storage (ADLS), you would like to register the file in your metadata with all relevant details like when did the file arrive, what is number of records in the file etc. 
3.  The file in the landing layer is moved into the raw layer or a data lake and loaded into some sort of table. And once the file is loaded you would like to know how many records were loaded. If the number of records in the file is the same as the number of records loaded by the process in the table. 
4.  Next to this you would also like to identify the changes in the files received and would take a corresponding action for the same. Either you would update all records in the target table with the changes or you would like to identify the changes and mark it as active record in the target while close the old records. You would do, what we call a Slowly Changing Dimension, with either a Type 1, Type 2 or Type 3. 
5.  You would also like to do a data quality check on top of the data extracted from the source and sent to the landing layer. You would like to check the data types, null values, data constraints etc. to make sure that you have the correct data available from the source to be used for your use cases. 
6.  Additionally, you would also like to keep a track on the changes in the schema of the source and may be that you would also like the data pipeline to not fail in the event of changes to the source schema. 

# The Data Platform

To start with, lets talk about the design of a typical data platform. At the moment, my focus is on building the so called medallion architecture, using which the idea is to organize the data within a Lakehouse in bronze, silver and gold layers and the quality of data improves as it moves from one layer to another. Also the idea is to build a platform which can be used for both Batch and Streaming use case. In fact I should say that build a data platform which can support Event Driven Architecture.

![Medalion-Architecture.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658824305887/eMMkrLFnW.png?auto=compress)

**Raw Zone**:
This layer ingests the data from the source in the native format. There is no schema needed and we just dump all the data from the source into the Raw Layer either in form of csv files. 

**Curated**:
This layer picks up all the files from the Raw Zone and defines a structure, enforces schema, evolves schema as needed and usually tested for data quality. The data is in a format which can be natively queried and accessed for analysis. 

**Data Hub**:
I usually call this layer as a snapshot of the source but in a standardized form. Now by this I don't mean building academic enterprise data models but approach it pragmatically and build a data model which works for your organization. Don't fall into the trap of industry standard data models. This layer usually serves as the integration layer for multiple sources and also exposes the data to the whole organization which can be used for every kind of use case, whether it be a report or a machine learning mode.   

# Problem Statement

We have sources which are generating data in different frequencies. Few sources are once a day, some are every few hours and few sources are really real time emitting data every few seconds. You can have hundreds of files coming in every now and then and you need a process to keep a track of the files being processed and making sure you process a file exactly once. 

# The Autoloader Way

Now, this is where I feel AutoLoader can come in handy to incrementally process the files being extracted from the source and dumped in the landing layer in an Event-driven fashion. I could quickly get going with setting up AutoLoader and ingest the CSV files which were arriving without any set frequency. Let’s look into some of the features of Databricks Autoloader and their functionalities.

Below is the screenshot of the function which I created to process the data using Databricks Autoloader. At the end of this blog, you can also find the link for the notebook in my github repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1659029470333/uSLjgOxqt.png?auto=compress)

Auto Loader provides a Structured Streaming source called cloudFiles which when prefixed with options enables to perform multiple actions to support the requirements of an Event Driven architecture.

The first important option is the .format option which allows processing Avro, binary file, CSV, JSON, orc, parquet, and text file. There is no default value for this option and you need to specify the format of the file as a required option.

    .option('cloudFiles.format', source_format)

The option useNotifications allows you to choose whether you want to use the file notification mode or directory listing mode for detecting new files. If useNotifications is set to true, you need to provide the necessary permissions to create cloud resources. You can see the notifications being created in the Events section of Azure Storage once you start the job. Autoloader automatically sets up a notification service and queue service that subscribe to file events from the input directory. File notification mode is much better and recommended for a high volume of files coming into your landing zone.

    .option("cloudFiles.useNotifications", "true")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1659029247186/EaeTVcU00.png?auto=compress)

I also wrote a small piece of code that identifies on the basis of the key columns, if the data is already present in the table. If the keys along with the records are present in the target table and there is a change in the record, the code will update the existing record in the target delta lake table. If the record does not exist against the keys, it will insert the record as a new record into the target delta lake table. 

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1659029369530/qUMtADr-W.png?auto=compress)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1659029336514/TS\_S8xLlp.png?auto=compress)

The .foreachBatch option allows you to specify a function that is executed on the output data of every micro-batch of the streaming query. So basically you can define an action as a function and this option will execute that option before loading the data into your delta table.

Auto Loader keeps track of discovered files in the checkpoint location using RocksDB to provide exactly-once ingestion guarantees. You can use the checkpointLocation option to specify the path.

    .foreachBatch(upsertToDelta)

Now I loved this option of .trigger where you can use Autoloader for both batch and streaming use cases. With this option set to true, your Autoloader job can be configured to start, process the files available in the source location and then stop. This helps you keep a check on the cost and you don't need to have the cluster up and running all the time. If you know when your files are going to arrive, you can orchestrate this with a time-based schedule.

    .trigger(once=True)

Auto Loader keeps track of discovered files in the checkpoint location using RocksDB to provide exactly-once ingestion guarantees. You can use the checkpointLocation option to specify the path.

    .option("checkpointLocation", "/tmp/sky/customers/_checkpoints")

Once you are ready, you can execute the Autoloader function with the parameters defined at the runtime. Now in the below example, I used the .tirgger option set to true, so the job ran and loaded all the data in target and applied the SCD Type 1. 

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1659029336514/TS\_S8xLlp.png?auto=compress)

So any record which changed was updated in the target and all new records were added. The graph is pretty neat and works in real-time. So you can drop files in your source storage location and it will show up in the graph as Autoloader tries to process the files.

If you click the tab of Raw Data, you will find the below output in JSON format. which has a mine of information that can really help you in building a Data Observability Dashboard. It provides you the operational metadata with details of when the batch was run, how many records were processed, and some other information. To be honest, I still need to figure out how can I put this information to use. 

    {
      "id" : "7b9661ea-3438-4b2f-b41e-809043958ab0",
      "runId" : "1bd6e990-0b08-451c-be46-29c00f6b8233",
      "name" : "Merge New Data",
      "timestamp" : "2022-07-08T14:40:54.500Z",
      "batchId" : 7,
      "numInputRows" : 0,
      "inputRowsPerSecond" : 0.0,
      "processedRowsPerSecond" : 0.0,
      "durationMs" : {
        "latestOffset" : 0,
        "triggerExecution" : 0
      },
      "stateOperators" : [ ],
      "sources" : [ {
        "description" : "CloudFilesSource[/mnt/landing/customers/]",
        "startOffset" : {
          "seqNum" : 9,
          "sourceVersion" : 1,
          "lastBackfillStartTimeMs" : 1657033622925,
          "lastBackfillFinishTimeMs" : 1657033623542
        },
        "endOffset" : {
          "seqNum" : 9,
          "sourceVersion" : 1,
          "lastBackfillStartTimeMs" : 1657033622925,
          "lastBackfillFinishTimeMs" : 1657033623542
        },
        "latestOffset" : null,
        "numInputRows" : 0,
        "inputRowsPerSecond" : 0.0,
        "processedRowsPerSecond" : 0.0,
        "metrics" : {
          "approximateQueueSize" : "0",
          "numBytesOutstanding" : "0",
          "numFilesOutstanding" : "0"
        }
      } ],
      "sink" : {
        "description" : "ForeachBatchSink",
        "numOutputRows" : -1
      }
    }

To summarise, Databricks AutoLoader incrementally and efficiently processes new data files as they arrive in cloud storage without any additional setup. It supports both batches as well as streaming ways of ingesting data into the platform and I would definitely recommend using it if you are using Databricks to build your data platform. It takes away the need and complexity to build a data ingestion framework with all the features I have described earlier in this blog. Click here to access the Databricks notebook which I have created and used above. Additionally, I would love to hear your thoughts on this topic.

# References

*   [Official Document - Azure Databricks ](https://docs.microsoft.com/en-gb/azure/databricks/ingestion/auto-loader/)
*   [TLC Trip Record Data](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
*   [Medallion Architecture](https://databricks.com/glossary/medallion-architecture)