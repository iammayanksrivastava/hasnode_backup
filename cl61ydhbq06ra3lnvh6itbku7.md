## Getting Started with Databricks Delta Live Tables

# Background

One of the most common use cases we have had in the ETL workloads is tracking which incoming files have been processed and incremental processing of the new data coming in from the sources. During early days of career, I still remember we had this File Watcher component in our framework which used to keep watching for the files in the landing folder. The file watcher used to register each file coming into the landing layer and once all the required conditions were met the component used to process all the data into the DataWarehouse.