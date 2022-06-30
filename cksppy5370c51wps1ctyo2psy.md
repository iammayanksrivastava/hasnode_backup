## Real-Time Reporting with Azure


Real-time data ingestion and making use of the data for creating reports for the business users have been on my mind for quite some time. As an Architect, I have always believed that the time to report between the moment data is captured in an organization and the time it is available for users to use it for taking decisions should be as less as possible. With the data in hand, it always becomes easy for business users to make informed decisions.

Working with multiple businesses in the last couple of years, there have been discussions and counter-arguments on

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788419328/VPStC-gp9.jpeg)
> # Do we really need Real-Time Reporting”?

I would say if you can capture events from your source systems in real-time, then WHY NOT? One of the major benefits which can be achieved by Real-Time Data ingestion & Integration is Data Quality Assessment. The sooner you get hold of your data and assess it for Data Quality, the sooner you can fix it in your source system and the lesser would be a panic situation when you really need the data for your report. At the moment you want to use the data for reporting, you don’t want to spend time in making sure your data is correct, rather you would want to spend time in taking decisions on the basis of what you see on the report.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788421323/7HfV6UAfH.png)

Well, Data Quality is not the only reason why one should go for Real-Time Reporting. With strong business acumen, you can figure out early signals of trouble in your business and plug the gaps to stop being chaos and havoc at the business period. You don't want to be saying at the end of the month that you could have taken corrective actions if you knew this was coming. Real-time trend analysis is quite a powerful way to plug the problem before it happens. Real-Time Data ingestion can be a perfect use case for detecting Money Laundering, credit card, or banking frauds as well for the customer's next best actions and in real-time, you can propose a product to the customer depending on his browsing behavior or spending pattern.

In this post, my aim is to brainstorm on the ways we can do Real-Time reporting using the Microsoft Azure Stack. In the above diagram, you can see I have two patterns. Let me elaborate on the patterns in detail.

But before we go into each of the patterns, please note that I have taken the liberty to choose **Confluent Kafka** as the Event streaming platform. Confluent Kafka helps to capture database changes, IOT events, log events, or any other streaming data. So as a first step, assuming that we have databases like Oracle, MySQL, etc as source systems and streaming platforms like IoT devices or social media patterns, I can use Kafka to capture the changes and stream the data being created on social media platforms. Now Kafka is an option that I intend to use in all of my patterns and create an Enterprise Data Hub with all of the data being captured in real-time and can be used by multiple parties within the organization. Confluent Kafka provides you connectors for multiple databases like Oracle, MySQL, SQL Server as well as platforms like Twitter, Salesforce, MQTT, etc.

## Pattern 1

* In this pattern, first, we can capture the events from the underlying source systems or from streaming platforms into the topics within Confluent Kafka.

* Azure DataBricks can then be used to read from the Kafka topics in real-time and transform the data as per the business logic. Read data from Kafka topics into a spark dataframe and then apply the transformation logic to the data.

* The transformed data can then be written into the target data model within the Azure SQL Database.

* Connect PowerBI to Azure SQL Database and get access to data ingested and transformed at a real-time and build reports.

## Pattern 2

* In this pattern, we re-use the Confluent Kafka setup we created in Pattern 1. The data from the source system databases and the streaming platform get ingested into Kafka Topics. Azure DataBricks can read the data from the Kafka Topics into data frames.

* However, in this pattern what we can do is write the data first to Azure Data Lake Store in the form of files. Azure DataBricks can write data to ADLS and we can store all the raw data in form of files in ADLS to create a Real-Time Data Lake. Both Structured and unstructured data can be stored by using this pattern in the Azure Data Lake Store.

* Now on the basis of your end-user business Data requests, Azure DataBricks can pick data from the Azure Data Lake Store and transform it as per the target data model. In this case, also, I would use the compute power of Spark to transform the data.

* As a next step, I would use Azure DataBricks to write data into the target data model within the Azure SQL Database. All of the data is being ingested, transformed and loaded into the target data model with a near-real-time latency.

* Finally, business users can connect PowerBI to Azure SQL Database and create reports. The reports will be produced with a near-real-time latency to help business users make informed decisions.

If you think I have missed something or you think there is a more effective and better way of doing this, please feel free to comment on my post. Will be happy to take your feedback and incorporate in my post. Also, as a next step, I intend to do Proof of Concept on each of the patterns. So if you intend to collaborate with me on this, do let me know on my email *mayank.srivastav84@gmail.com*

**References**:
[**What is Azure Event Hubs? - a Big Data ingestion service**
*Azure Event Hubs is a big data streaming platform and event ingestion service. It can receive and process millions of…*docs.microsoft.com](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about)
[**What is Confluent Platform? - Confluent Platform**
*Confluent provides the industry's only enterprise-ready Event Streaming Platform, driving a new paradigm for…*docs.confluent.io](https://docs.confluent.io/current/platform.html)
[**Structured Streaming + Kafka Integration Guide (Kafka broker version 0.10.0 or higher)**
*Structured Streaming integration for Kafka 0.10 to read data from and write data to Kafka. For Scala/Java applications…*spark.apache.org](https://spark.apache.org/docs/2.2.0/structured-streaming-kafka-integration.html)