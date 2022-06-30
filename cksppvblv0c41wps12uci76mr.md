## Loading a csv file into Azure SQL Database from Azure Storage.


Hassle free and Quick way of loading an csv file into Azure SQL Database

![Photo by [Boitumelo Phetla](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788286203/l7AYratrW.html) on [Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)](https://cdn-images-1.medium.com/max/12000/1*jxyqXlAsH2ky6xPDfJVNsg.jpeg)*Photo by [Boitumelo Phetla](https://unsplash.com/@writecodenow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

Over the weekend, I wanted to do a quick proof of concept on certain capabilities of Databricks and I wanted to use Azure SQL as a source. I faced quite bit of challenges and google was not kind enough to provide me a solution. I tried everything from bcp to bulk insert the file on my local computer and somehow it came out with errors which failed to fix.

Finally I managed to load the csv file into database and thought of sharing this with everyone, so that if you have to quickly load data into Azure SQL Database and you don’t want to write a script in Databricks or use Azure Data Factory for such a simple task. And before you move ahead, I am assuming that you have a fair understanding of the Azure ecosystem particularly the Storage Account.

So first things first, upload the file, you want to load into Azure SQL database, to a container in Azure Storage Account. You can use the normal Blob container and don’t have to use Azure Data Lake Storage for this.

![Screenshot from Azure Blob](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788287630/0QPRrrixR.png)*Screenshot from Azure Blob*

Second, you need to create a Shared Access Signature for the storage account

![Screenshot from Azure Storage Account](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788289351/wNjHAjX7H.png)*Screenshot from Azure Storage Account*

Now go to the Azure SQL Database, where you would like to load the csv file and execute the following lines. Please replace the secret with the secret you have generated in the previous step. Also, please make sure you replace the location of the blob storage with the one you

```
*CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';*

CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = '******srt=sco&sp=rwac&se=2017–02–01T00:55:34Z&st=2016–12–29T16:55:34Z***************';

NOTE: Make sure that you don't have a leading ? in SAS token, and that you have at least read permission on the object that should be loaded srt=o&sp=r, and expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
LOCATION = 'https://****************.blob.core.windows.net/adventureworks'
 , CREDENTIAL= MyAzureBlobStorageCredential
```


Once you have done the above steps, you are left with only one last step and that is to insert the file into the table. Please remember to create the table in the database before you load the file and also remember to keep the schema of table exactly as the file structure, else you might run into errors. And the last step is

```
BULK INSERT [dbo].[lnd_lending_club_acc_loans] FROM 'accepted_2007_to_2018Q4.csv'
WITH (
    CHECK_CONSTRAINTS,
    DATA_SOURCE = 'MyAzureBlobStorage',
    DATAFILETYPE='char',
    FIELDTERMINATOR=',',
    ROWTERMINATOR='0x0a',
 FIRSTROW=3,
    KEEPIDENTITY,
    TABLOCK
);
```


And if you have done everything correct, you can expect the table to be loaded with the data in the csv file. There are many other ways of doing this, but this one worked for me and was quite quick.