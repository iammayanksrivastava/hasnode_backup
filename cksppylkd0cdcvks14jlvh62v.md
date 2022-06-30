## Securing Azure Storage Account


Photo by Steve Johnson on Unsplash

Azure Blob storage is an object storage solution for the cloud. Blob storage is optimized for storing massive amounts of data and you can use it for storing both structured and unstructured data like .csv, text, parquet, .json files and audio, video, and images.

While using Azure Storage account in an enterprise, you would always want to secure your storage account so that it is compliant as per your internal IT policies as well as regulatory compliance.

I won’t go into further details of the Storage Account here but would straight come to the point which I want to discuss here.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788427878/kEHWV701Z.jpeg)
> # “Why should we secure my Storage Account and How should we secure my Storage Account?”

There are multiple things that you can do to secure your Storage Account. But for a completely secured Storage Account, one should apply a combination of all the below methods

* **Firewall and Network — Where can the Storage Account be accessed from?**

* **Access Authorization — Who can access your Storage Account and how?**

* **Encryption**

* **Monitoring — What’s happening in your Storage Account?**

* **IT Resiliency — What if the Storage account becomes inaccessible due to outage?**

Let’s go through each of the points listed above one by one to understand how can we secure a Storage Account

## Firewall and Network

The first thing which I would do is enable the option “Secure Transfer Required” while deploying the storage account. A very basic one, but this enables access to the storage account only using HTTPS. In addition to this, also ensure secure TLS for Azure Storage at the client-side before sending requests to the Azure Storage service.

![Enabling Secure Transfer Required](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788429609/JbZgBSRft.png)*Enabling Secure Transfer Required*

Secondly, restrict the access to your storage account from select networks and not all networks. You should limit the access to the storage account from a specified IP address, IP ranges or a list of subnets configured in the Azure Virtual Networks.

![Authorize access to Storage Account from specified network](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788431231/e5nJ2dW4O.png)*Authorize access to Storage Account from specified network*

One thing which you will notice here is the small checkbox which says “Allow Trusted Microsoft Services to access this Storage Account”. Below are the services which access the Storage Account and you have to take into account before deciding to check that box.

![Reference from Linux Academy course for AZ-103.](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788433208/IKmWV6i_C.png)*Reference from Linux Academy course for AZ-103.*

## Authentication and Authorization

Authentication and Authorization facilitate controlling access to the data stored within the Storage Account via the portal, CLI or REST API when accessed by the end-users like business users, developers or operations team.

Usage of access keys in Storage Account should strictly refrain since it’s like root access with the permissions to do everything and anything on the storage account. Upon deployment, you should store the access keys in the Key Vault and an Automation runbook should be configured to rotate the keys frequently

Share Access Signatures can be used for allowing access to the users to interact with the storage account since you can control what a user can do with the storage account.

Role-based access control (RBAC) to control access to the storage account. Azure provides inbuilt RBAC roles for authorizing access to blob and queue data using Azure AD and OAuth.

Azure Active Directory should be used to authenticate and authorize access to the Storage account, with the new entry to the block being the Managed Identities which can be used to authorize access to Storage Account from other Azure applications which can remove the need of storing credentials For example, in order to authorize access on Storage Account to Azure Data Factory, you can use managed identities which uses Active Directory to authenticate and authorize the resources.

## Encryption

Encryption is required for two things — Encryption of Data at rest which means data is encrypted when stored and Encryption of Data in transit which means data is being encrypted during the movement from one place to another.

By default all the data stored in the Storage account is encrypted by default in Azure and the data is decrypted automatically when accessed. However, you can also use your keys to encrypt the data. Please note that in case you intend to use your keys, please use Azure Key Vault to store and manage your keys.

![Encrypting Data at Rest](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788434981/i0ozfxBzA.png)*Encrypting Data at Rest*

## Monitoring

Monitoring should be a de-facto for every component you deploy on Azure if you are dealing with an enterprise-grade production environment which deals with a lot of sensitive data. Having worked for Financial institutions, I have always been spooked by applications which are not being monitored. Azure Monitor is the native Monitoring solution within Azure, even though you can also forward your logs to a central monitoring solution like Splunk. Apart from enabling monitoring for your storage account, also enable Alerts in response to events on the Storage Account.

Configure Monitoring for a Storage Account by enabling Diagnostic settings in the Storage Account.

![Diagnostic Settings for Storage Account.](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788436477/dJZhXvad6.png)*Diagnostic Settings for Storage Account.*

Additionally please create dashboards based on metrics to monitor the usage of the Storage Account. You can configure the dashboards based on the metrics you want to monitor. Also, please configure Action groups and Alerts to send out communication when a certain event happens on your Storage Account, one of the examples being malicious access to the Storage Account.

![Configure Alerts for your Storage Account](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788438145/Mltnd6G7p.png)*Configure Alerts for your Storage Account*

## IT Resiliency

High Availability and Backup are the two main things to be kept in mind to make sure you have a Resilient storage solution. It is to ensure that due to Infrastructure outage or accidental deletes, you don’t get knocked out from accessing your data because that can be chaotic for a business.

One of the things which you should do is configure Geo-replication for your storage account. It is advisable (keep in mind the regulatory requirements for your country) to configure Read access Geo-Redundant replication for your storage account so that your data is copied to a secondary location, to which you can initiate a failover, and data can be accessed in event of a transient hardware failure, network or power outage or natural disasters. In event of a failure of a primary endpoint, you can still read the data from the secondary endpoint.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788439740/sityHnnHm.png)

When configuring a storage account, it is always advisable to enable Soft delete option which helps recover blob data in an event where Blobs or blob snapshot are deleted. You can also configure retention policies

![Enable Soft Delete](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788441570/pbs-_uFnj.png)*Enable Soft Delete*

*If you think I have missed something which can make the Storage Account more secure, please feel free to comment. It will be a learning for me and I will make sure I add it to the list and of course you will be credited for sharing.*

*PS: Special thanks to [Marco *Kerstens](https://www.linkedin.com/in/marco-kerstens-78328820/) [@kerstensgaming](http://twitter.com/kerstensgaming) for his valuable inputs !!