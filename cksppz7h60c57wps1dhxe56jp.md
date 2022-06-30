## Deploying Network in AWS using Terraform.


Photo by Petter Lagson on Unsplash

Terraform is one of the most important tools in my tech stack to deal with the Cloud infrastructure. Gone are the days when I would use the mouse to click-click and deploy a network infrastructure or VM instances.

In this post, I would like to share my experience in deploying a very simple but Highly available network infrastructure. By High availability, I mean that we have a network with subnets in two availability zones in one region. So just in case one of the Availability Zones goes down, we will have our application running in the second availability zone.

This is a very simple example where I will show how to deploy a network using Terraform. In the next post, we will use this network to deploy a web server and a database in this network and make the application highly available using Load Balancers.

## **Deploy the environment in AWS**

In order to deploy the above configuration in AWS using, we will create the following files:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788448980/bcgYVKtak.png)

**Terraform.tfvars**: Terraform files have an extension .tf. The variables.tf file will contain the variables and their values which will be used in the main configuration file. The variables file will look something like below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788450461/hWDBU2GUE.png)

**main.tf**: This file will contain the configuration which will be deployed to create the network infrastructure in AWS. For the sake of this demo, we will create the following components in AWS.

First, we need to call all our variables from the variable file we created in the first step. We can call it as follows:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788452289/ZxSdJU3bMf.png)

Secondly, the provider needs to be configured with the proper credentials before it can be used. Please note we will use the format ${var.variable_name} to call the required variables declared in the first step.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788453811/Tr9Xkk7Hn.png)

Next, we create a VPC (Virtual Private Cloud) in AWS. A VPC helps the end-user isolate resources within the AWS and restrict access to other users or VPC’s within AWS. Think of VPC as a Virtual Data Centre. In this example, we will create a VPC with CIDR block of 10.0.0.0/16 in eu-west-2 region (London) with a default instance tenancy.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788455297/W5GWaU_sMr.png)

Next, let’s attach an Internet Gateway to enable internet traffic in and out of the VPC. The Internet Gateway is attached to the VPC and is a highly available and redundant VPC component. Internet gateway also enables between instances in your VPC.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788457031/iLvZEjSE7.png)

Now with VPC and Internet gateway in place, its time to create subnets within the availability zones in the region where these components have been deployed.

In order to dynamically deploy subnets, we will first identify the availability zones in the EU region. We don’t intend to hardcode the Availablity zones. We will use the Data Source modules in Terraform to query the existing resources of AWS. In this case, we will use the data module to query the availability zones in the region.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788458533/Gg4_O6NqH.png)

Now, the eu-west-1 region will come back with 3 availability zones. Now we want our network to be only in 2 zones. We can create subnet using the interpolation syntax in terraform. First, we define the count of subnets which will throw a number 3 and we can use -1 to specify that we need two subnets. Please take note of the way we use interpolation syntax for defining the availability zones and CIDR blocks for each subnet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788460160/UQ0fbL1sq.png)

Finally, we need to create a route table and associate it to the subnet for allowing internet traffic on the instances in the subnet. We will point the target of the route 0.0.0.0/0 to the Internet gateway we deployed earlier.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788461912/c6R_iJRoi.png)

Once the code is completed, let's execute the code using terraform apply command. If everything is correctly coded, the apply command will list all the resources which will be created. Type yes and hit enter, sit back and have a coffee while Terraform does all for you. In total seven resources should be created. 1 VPC, 1 internet gateway, 1 public route, 2 Public subnets, and 2 route table associations.

In addition to the above resources, a default security group and a NACL will also be created in the VPC. In the next post, we will create some explicit security groups and NACL. But for now, let’s live with this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788464523/CUVB-FhiA.png)

After the process successfully completes, it will tell you how many resources were added into the AWS. You can verify the details by logging into the console.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788466497/vQrwznXux.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788468517/9OseUJx2M.png)

Please let me know if you have any questions or doubts. Also feel free to let me know if something could have been done in a more efficient way.

You can find the code for this demo on git hub. Please click on this [**link](https://github.com/mayanksrivastav84/CI-CD/tree/master/Terraform/aws_terraform/create_multi_az_vpc)**.

In the end, you can also create a visual graph of the resources created using Terraform. The below command can be used for the same
> terraform graph | dot -Tsvg &gt; terraform.svg

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788470414/wVEEHBLO8.png)