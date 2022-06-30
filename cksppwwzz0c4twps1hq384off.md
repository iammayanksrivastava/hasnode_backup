## Five mistakes to avoid while building a Data Platform


Why is it important to have strong fundamentals for a Data Platform?

Having spent a couple of years now in the world of data, building end to end data management platforms right from the days, in the early 2000s, of Traditional databases like Oracle and SQL Server to the recent, days of Hadoop and real-time analytics using Databricks and Kafka, I have realized that, everywhere the problem statement is same and unfortunately the mistakes we make during the process of building the data platform are also the same.

So go ahead and read what I think are the top five mistakes we should avoid while building a Data Management Platform. Feel free to add more in the comments sections, from your experience.

![Photo by [Sarah Kilian](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788364201/-BLsBH4Y4.html) on [Unsplash](https://unsplash.com/s/photos/oops?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)](https://cdn-images-1.medium.com/max/12000/1*8xZkF6mq2knNGxbaAHt2sw.jpeg)*Photo by [Sarah Kilian](https://unsplash.com/@rojekilian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/oops?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

**Generic Frameworks? That’s not cool**

With all kinds of fancy technologies around, at times we get carried away to write as many lines of codes as possible. It can be fun indeed, but the focus in operationalized scenarios should be on building configuration and not writing complex ETL codes to source and transform data. Avoid having multiple codes for each configuration. In fact, make two or three generic code frameworks for each step which can process all the configuration.

The development of these frameworks would consume sometimes initially, but gradually it would be just about creating configurations that would reduce the time to delivery.

**Data quality is not our job — We just deliver data..Right?**

Yes and No. Yes, the most important job of a data platform is to provide data. But trust me, and I have seen this in the past, your data platform will be as good as a garbage box which no one would use if you don’t have the right quality of data in your system. Now, yes we don’t fix the data in our data management platforms and the guiding principle should be to fix the data at the source, but you will have to be transparent on the quality of data being stored in your platform. Assess the data for the quality, make the DQ issues transparent on a dashboard, and establish a feedback loop with the sources to fix the issue. And yes, own it.

**Metadata Management — Is that a migratory Bird?**

That's one of the worst things you can do while building a data management platform — Ignore Metadata management. Remember, your data will be used by business users who don’t understand technology the way an IT team can. So metadata forms the most important part if you want your initiative to be a success story. The business should have access to the data catalog which not only tells them what is there in your system but also explains what a particular item means and where does it come from. Functional and Technical metadata should be maintained with the highest levels of accuracy to ensure your consumers use your data platform in their decision-making process.

**Duh !! What value will the business add? This is an IT Delivery**

Now that's a mistake I have seen both the Business and IT teams make. We think building a data management platform is an IT thing and businesses won't be able to understand anything during the process. But that’s not right. Business teams should be involved in every step of the process to build the data platform and everything you build should be solving one or the other business problem. Attach a business value to everything you deliver and make sure the business gets a chance to ask questions. Trust me, I have seen personally, that in this process at time business comes up with questions which help in increasing the value delivered out of the platform. Also, it helps in fixing issues early in the process rather than in the User Acceptance testing.

**Data Governance? Really? Who cares?**

Data Governance is not a one-man job. It's not the process of finding one person and giving him all the tasks of governing the data. We are in an era of data management, where we need to move from a reactive and isolated approach to a more proactive and collaborative approach to Data Governance. While building a data platform, ensure data has been defined for its criticality, ownership, and remediation process in event of an issue with the data. Data Governance is not only about Data Quality issues and handling them, it's about every aspect of managing information that leads to the end target of having an end to end data management platform. Personally, I keep the DAMA Data management Book of Knowledge in my pockets when I deal with building Data management platforms. Have a look and it will be worth your time and money.

**Conclusion**

It is really important to build strong fundamentals of your Data Platform to avoid it becoming a legacy in five years and help businesses in using your data platform for data-driven and well-informed decisions as well as trust the data being sourced into your platforms. Metadata and Quality should be at the core of any Product Owner building a Data platform and not push these items to the bottom of the backlog.