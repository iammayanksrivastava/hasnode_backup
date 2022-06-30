## Diagram as a Code for Cloud Architects


Photo by Sven Mieke on Unsplash

Yes, you heard it right. We were not getting over with the Infrastructure as code and now we are already talking about Diagram as a Code.

As an Architect, I have always believed that we should be able to write code too, so as to create a prototype of my designs and prove that what we preach does work. I jokingly call myself an “Architect who can code”

My take on the concept of a Diagram as a code? It would be extremely useful and fabulous to simply write a few lines of code that generate a Cloud Architecture Diagram. However, there is still a way to go before I can adapt it fully in my day to day life as an Architect.

I stumbled upon [https://diagrams.mingrammer.com/](https://diagrams.mingrammer.com/) while googling around on the internet looking for some interesting items to learn and it took me not more than 10 minutes before I could get started and created my first Diagram as a code.

Installing Diagrams on a MacBook was easier than brewing a cup of coffee from a Nespresso machine. The documentation on the website was easy to follow. Also, the GitHub repository of the product allows you to peep into their code and see what classes they have to offer. There are not many examples, but playing around is easy peasy if you know a little bit of coding.

Installation of Diagrams took me two commands, apart from having Python installed on my laptop. You have to use Python3 to use Diagrams. There is a dependency on GraphViz, so make sure you have it installed too.
> *brew install graphviz*
> *pip3 install diagrams*

So, after the installation of the Diagrams, I just had to write a few lines of code. For my first Diagram as a Code, I decided to create an Architecture Diagram to build a Data Standardization Platform. Following are the key pointers:

* We have two sources, a MySQL source, and a SQL Server sources

* Both sources send data to an Azure Blob Storage

* Data from Blob Storage is picked up by an Ingestion Framework and loaded into Azure Data Lake Store.

* Data from ADLS is picked up by the ingestion framework in Data Factory and loaded into the Cleansing layer built on top of SQL Database

* Data from the Cleansing layer is picked up by Databricks, transformed, and loaded into the Harmonized layer built on top of Azure SQL Database.

It took me only 10–15 minutes and not more than 35 lines of code to incorporate the above requirement and create an Architecture Diagram to be implemented.

![Image by Author](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788275549/O5WKAs70E.png)*Image by Author*

The learning curve for Diagrams is not much if you already know how to code in Python and understand the basics of programming. If I can do it, I am sure anyone can do it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788277236/e9WQWBQ9z.png)

![Image by Author](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788278610/9Ru_Dp3S9.png)*Image by Author*