## Getting Started with Databricks Autoloader

# Background

One of the most common use cases we have had in the ETL workloads is tracking which incoming files have been processed and incremental processing of the new data coming in from the sources. During early days of career, I still remember we had this File Watcher component in our framework which used to keep watching for the files in the landing folder. The file watcher used to register each file coming into the landing layer and once all the required conditions were met the component used to process all the data into the DataWarehouse.

Even though a lot has changed in last 15 years in terms of technology and we have moved from strictly on-prem to "Let's build Data Platform on Cloud" strategy, few basic concepts are still the same and form the core of any data platform. Even till date we have the same problem statement where we receive the data from the source systems which we would like to process incrementally and also achieve the guarantee of processed exactly one for every file.  

This is a multi-part blog and I will be covering Autooader, Delta Live Tables and Workflows in this series. This is the first part of the series where I cover Databricks AutoLoader. 

# Ingesting Data from Source

Typically, we all would like to have an Event driven orchestration and below is what you would like to do: 

1.  Source data is extracted by a Data Extraction pipeline and dumped into a landing layer. You would expect the pipeline to extract data incrementally every time there is a change at the source. So you would expect a file per day or many times a day. May be a file with all changes every couple of minutes or even a minute. 
2.  The moment the file lands into the landing layer, which can be typically a object based storage like an Azure blob or Azure Data Lake Storage (ADLS), you would like to register the file in your metadata with all relevant details like when did the file arrive, what is number of records in the file etc. 
3.  The file in the landing layer is moved into the raw layer or a data lake and loaded into some sort of table. And once the file is loaded you would like to know how many records were loaded. If the number of records in the file is the same as the number of records loaded by the process in the table. 
4.  Next to this you would also like to identify the changes in the files received and would take a corresponding action for the same. Either you would update all records in the target table with the changes or you would like to identify the changes and mark it as active record in the target while close the old records. You would do, what we call a Slowly Changing Dimension, with either a Type 1, Type 2 or Type 3. 
5.  You would also like to do a data quality check on top of the data extracted from the source and sent to the landing layer. You would like to check the data types, null values, data constraints etc. to make sure that you have the correct data available from the source to be used for your use cases. 
6.  Additionally, you would also like to keep a track on the changes in the schema of the source and may be that you would also like the data pipeline to not fail in the event of changes to the source schema. 

# The Data Platform

To start with, lets talk about the design of a typical data platform. At the moment, my focus is on building the so called medallion architecture, using which the idea is to organize the data within a Lakehouse in bronze, silver and gold layers and the quality of data improves as it moves from one layer to another. Also the idea is to build a platform which can be used for both Batch and Streaming use case. In fact I should say that build a data platform which can support Event Driven Architecture.

![Medallion](Medalion-Architecture.png)

**Raw Zone**:
This layer ingests the data from the source in the native format. There is no schema needed and we just dump all the data from the source into the Raw Layer either in form of csv files. 

**Curated**:
This layer picks up all the files from the Raw Zone and defines a structure, enforces schema, evolves schema as needed and usually tested for data quality. The data is in a format which can be natively queried and accessed for analysis. 

**Data Hub**:
I usually call this layer as a snapshot of the source but in a standardized form. Now by this I don't mean building academic enterprise data models but approach it pragmatically and build a data model which works for your organization. Don't fall into the trap of industry standard data models. This layer usually serves as the integration layer for multiple sources and also exposes the data to the whole organization which can be used for every kind of use case, whether it be a report or a machine learning mode.   

# Problem Statement

We have sources which are generating data in different frequencies. Few sources are once a day, some are every few hours and few sources are really real time emitting data every few seconds. You can have hundreds of files coming in every now and then and you need a process to keep a track of the files being processed and making sure you process a file exactly once. 

# The Autoloader Way

Now this is where I feel AutoLoader can really come handy to incrementally process the files being extracted from the source and dumped in the landing layer in an Event driven fashion. I could quickly get going with setting up AutoLoader and ingest the csv files which were arriving without any set frequency   

Databricks AutoLoader incrementally and efficiently processes new data files as they arrive in cloud storage without any additional setup. It supports both batch as well as streaming way of ingesting data into the platform and I would definitely recommend using it if you are using Databricks to build your data platform. It takes away the need and complexity to build a data ingestion framework with all the features I have described earlier in this blog. 

# References

*   [Official Document - Azure Databricks ](https://docs.microsoft.com/en-gb/azure/databricks/ingestion/auto-loader/)
*   [TLC Trip Record Data](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
*   [Medallion Architecture](https://databricks.com/glossary/medallion-architecture)