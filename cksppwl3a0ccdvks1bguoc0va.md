## Creating a personal VPN Server using Google Cloud


A VPN works by routing your device’s internet connection through your chosen VPN’s private server rather than your internet service provider (ISP) so that when your data is transmitted to the internet, it comes from the VPN rather than your computer.

![Photo by [https://cdn.hashnode.com/res/hashnode/image/upload/v1629788310928/JATrwzOym.html](https://unsplash.com/@danny144)](https://cdn-images-1.medium.com/max/10368/1*QRYrbCDXcDmUU9fK66YgAA.jpeg)*Photo by [https://unsplash.com/@danny144](https://unsplash.com/@danny144)*

I was using NordVPN until recently and even though they have a strictly no-log policy but the only information we keep about our users is an e-mail address (used for connecting to VPN, marketing, and troubleshooting purposes) and the billing information (used for refunding procedures). But why should I even let them save that?

**Here comes Outline VPN !!**

As per the official website, Outline makes it easy for news organizations and journalists to set up and manage their own virtual private network (VPN), allowing them to connect to the internet securely and keep their communications private. With most VPN services, a third-party manages the VPN servers and may have access to the information going through them.

![[https://cdn.hashnode.com/res/hashnode/image/upload/v1629788312314/S_hkHL8Ho.html](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788320335/xLrOUTweP.html)](https://cdn-images-1.medium.com/max/5384/1*iDi3FzkZ1A6qeJabBdc43A.png)*[https://getoutline.org/en/home](https://getoutline.org/en/home)*

With Outline, you or your organization can set up and manage your own VPN servers, effectively putting you back in control of your own online privacy.

**Setting up VPN servers on Google Cloud**

This is an easy part. Go over to [https://console.cloud.google.com/](https://console.cloud.google.com/) and set up a VM instance under the Compute Engine section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788313719/JygmjuiXg.png)

You can use the below settings. The server can be a basic one since it will not need any resources for computing. Its the gateway for your VPN. I have created two servers one in India and another one in the US

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788315412/8IG1McRG8.png)

Add a network tag named “outline” to the VM. You can name it anything you want.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788317056/doz-HrucC.png)

As a next step, please go ahead and create a Firewall rule. Please note the name of the rule should be the same as the network tag you added to the VM. Add the IP range 0.0.0.0/0 to the filters in the network rule to allow traffic from all the IP’s.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788318758/IwpMjdpTJ.png)

Once you are done with the creation of the VM, we move to the next step of setting up the Outline VPN on the VM instances.

**Setting up Outline VPN**

The first thing you need to do is install the Outline manager on your laptop. You can go to the Outline website and download the outline manager.

![[https://getoutline.org/en/home](https://getoutline.org/en/home)](https://cdn-images-1.medium.com/max/6084/1*FRDaKjje_Ykyw1N1I-ge8Q.png)*[https://getoutline.org/en/home](https://getoutline.org/en/home)*

Post-installation of the app, you will get the below screen. Please note that using Google Cloud is my personal preference, but you can also use Digital Ocean, AWS lightsail, VULTR, Linode, Liquid Web, etc.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788321807/DqdOFbXoJ.png)

Once, you click on the Google Cloud as a preferred option, you will get the below screen

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788323687/yGVyNKGoI.png)

From here, you need to go back to the Google Cloud console and login to the VM. You can use the option “Open in a browser window”

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788325457/K31zX57Tg.png)

Once you log in, into the VM instance, you need to copy the command from the outline manager and paste that into the terminal of the VM instance. The command installs all required components, which also includes docker. I would go by the default options and after the installation, you would get an API URL key.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788327339/b0h1UH9Zz.png)

Copy the API key and move back to the Outline manager and paste the key on the third step as follows and hit the done button

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788329136/tpachEUlO.png)

If the setup is successful, you will get the below screen

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788330801/o5KuyWbqp.png)

**Connecting to VPN Server using Outline VPN**

Okay, so now you have the VPN server created and Outline VPN setup on the servers. Now as a next step, we need to connect to the internet using the VPN server. The outline provides a tool that you can install on your laptop and mobile to connect to the internet using VPN.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788332431/NlLs7d0Bk.png)

Once you install the tool and open the interface for Outline, you will need to add your VPN server. Click on the + icon on the right.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788334880/LZQ9TImGh.png)

Once you click on the plus (+) sign you will get a placeholder to add the key

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788336871/iAx7EPWKF.png)

You need to go back to the Outline VPN manager and get the access key for your VPN Server. Click on the icon market

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788338509/BoJM1AB6i.png)

Click on the Connect this device to connect your laptop via VPN

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788340293/G5vgr22zG.png)

You will get the below screen. Copy the access code

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788342227/2DaNvkqLW.png)

Click on Add Server. It will automatically add the server to your Outline app installed earlier.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788343987/6ioeKXDlN.png)

You can see the server added into your Outline app.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788346077/WLFShpjV6.png)

Hit the connect button and voila you are connected via the VPN server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1629788348286/kA_Dfrbnp.png)