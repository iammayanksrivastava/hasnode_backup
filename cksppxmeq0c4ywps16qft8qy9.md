## Running SQL Queries against Delta Tables using Databricks SQL Analytics


Photo by Luke Chesser on Unsplash

One of the most awaited features, which was released by Databricks in Data & AI Summit 2020 SQL Analytics, and got me excited and I was all “Hell Yeah !!!”

Over the last two years, I have been evangelizing Databricks in the Architecture & Solution Design for the Unified Data Platforms.

For a Data Engineer, Databricks has proved to be a very scalable and effective platform with the freedom to choose from SQL, Scala, Python, R to write data engineering pipelines to extract and transform data and use Delta to store the data. Databricks along with Delta lake has proved quite effective in building Unified Data Analytics Platforms for any scale of organizations.

![Image by Author](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788378373/Sglk7Iq3-.png)*Image by Author*

However, even though the adaptation of the Databricks and especially the Delta Lake has been quite encouraging within the Data Engineer community, the adoption of the platform in the Data Analysts community, who are proficient in writing SQL Queries, have not been very enthusiastic. No analyst would like to write Databricks notebooks and maintain them just to fire some SQL queries for a quick Data Exploration activity.

One of the features which had been lacking was the ability, for the Data Analysts with SQL skills to use a SQL editor like interface which they have been used to with the Databases like Azure Synapse or Azure SQL or Oracle or Microsoft SQL. But with the release of SQL Analytics, Databricks has plugged that problem as well.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788379885/WJPJ3-yj1.png)*Screenshot from Databricks SQL Analytics*

At the lower right-hand side corner, you have a new icon in the shape of a square with dots which when clicked gives you the option to access the SQL Analytics (SQL Editor and Dashboarding). The SQL Analytics not only lets you fire SQL queries against your data in the Databricks platform, but you can also create visual dashboards write in your queries.

When you click on the option of SQL Analytics, you will be taken to a new workspace that will look something like this. The look and feel of the new workspace are quite appealing. However, letting the Data platform owner customize the landing page would be a good add-on. Personally, I would like to add some descriptions and links to the documentation which can help the users understand the data in the platform.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788381478/gSSOwtyxS.png)*Screenshot from Databricks SQL Analytics*

SQL Analytics can be used to query the data within your Data platform build using Delta lake and Databricks. You can provide access to the Analysts community on top of your data in Refined and Aggregated layers, who can then run SQL queries, which they have been used to in the traditional database environments.

![Image by Author](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788383063/sDjI5qiFi.png)*Image by Author*

The first thing you need to do is create a SQL End Point. Click on the logo on the left-hand side which says Endpoints and then clicks on New SQL Endpoint to create one for yourself.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788384368/3d_0syoaK.png)*Screenshot from Databricks SQL Analytics*

A SQL Endpoint is a connection to a set of internal data objects on which you run SQL queries. It is a compute cluster, quite similar to the cluster we have known all the while in the Databricks that lets you run SQL commands on data objects within the Azure Databricks environment. One interesting thing which I see here is the option to enable Photon that lets you decide whether queries are executed on a native vectorized engine that speeds up query execution. I am yet to test this feature and will be doing that soon along with my tests on Adaptive Query Execution which has been enabled by default in the Databricks runtimes 7.0 and onwards.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788385913/jjUwGQtOe.png)*Screenshot from Databricks SQL Analytics*

Once you have created the SQL Endpoint, you can now go back and click on the Queries logo on the left-hand side of the workspace.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788387547/3mcXqh9sP.png)*Screenshot from Databricks SQL Analytics*

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788389042/KqFceOu5Xk.png)*Screenshot from Databricks SQL Analytics*

Click on New Query and this will open your favorite SQL Editor kind of interface. As you can see in the below screenshot, I had created a table in Delta using the Data Science and Engineering workspace which is also visible here in the left-hand panel.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788390626/kYgXCb3cm.png)*Screenshot from Databricks SQL Analytics*

If I click on the table, it gives me access to the schema of the table with the data types.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788392141/m70MVkO9g.png)*Screenshot from Databricks SQL Analytics*

On the right-hand side of the panel, I can simply write my SQL Query and hit execute or press Ctrl+Enter. Once I do that, if my SQL Endpoint compute cluster is not running, it will first warm-up and enable the cluster and then submit the query. Good thing is that the query editor is quite intuitive with the option of auto-complete enabled by default.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788393701/f7Nt0MIsw.png)*Screenshot from Databricks SQL Analytics*

Once the query has been executed, which is dependent on the configuration of your SQL Endpoint, the results would be displayed at the bottom of the workspace.

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788395267/9BhLPup_v.png)*Screenshot from Databricks SQL Analytics*

The query editor also has one more feature, which might sound not very interesting, but trust me it’s an important one and it is the format query option (Ctrl + Shift + F).

![Screenshot from Databricks SQL Analytics](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788396820/MU-XkmBVy.png)*Screenshot from Databricks SQL Analytics*

This is a very high-level overview of how can we use SQL Analytics for analyzing the data within the Databricks platform. There is more than just firing some SQL queries and we need to think of Administrative and operational governance on top of the platform. But this is a very good addition to the stack and will bring the Data Analysts community close to the Unified Data Platforms built using Databricks and Delta and can help in driving organizations towards data-driven decision making