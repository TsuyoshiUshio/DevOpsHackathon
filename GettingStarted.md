Getting Started
===========

This guide covers getting up and running an sample application for DevOps.
After reading this guide, you will know :

* How to create Continuous Delivery environment using VSO
* How to refer some actual example of Infrastructure as Code
* Where to find additional information for Hackathon

1. Overview
-----------
DevOps is a language-agnostic concept, however learning the whole practices are really difficult
unless you join a DevOps project. In this tutorial, you can learn about Continuous Delivery / Continuous Integration and Infrastructure as Code using Microsoft Azure and Visual Studio Online.

### 1.1. Architecture

You will use [PartsUnlimitedMRP](https://github.com/Microsoft/PartsUnlimitedMRP) application. It includes WebServer and API Server.
You are going to create a build server and production server. These are Linux(Ubuntu) servers. This application is written by Java / Javascript.


If you change the PartsUnlimitedMRP, Visual Studio online will automatically detect the change then
automatically build/test the PartsUnlimitedMRP on the bulid server. After the build has been finished,
Visual Studio online deploy the apprication to the production server.


If you want to try .NET application, you can use PartsUnlimited application.

2. Tutorial
-----------
You can learn Continuous Delivery / Continuous Integration and Infrastructure as Code in this tutorial.
Enjoy DevOps practice!

### 2.1. Prerequisite

Before starting this tutorial, you need to make sure to use Azure / Visual Studio Online / Azure SDK.

#### 2.1.1. Azure Pass

You can refer to [Azure Pass step-by-step guide](http://1drv.ms/1LIcy3E) to set up Azure Pass.

Now you can use Azure.

#### 2.1.2. Visual Studio Online

One of your team member should sign in Azure and create an Visual Studio Online account. Then they should
invite other members. Please make sure everyone in your team can Sign-in Visual Studio Online.

#### 2.1.3. Azure SDK

You can refer to this page. [Azure CLIとChefを始めるためのSubscription関係まとめ](http://qiita.com/TsuyoshiUshio@github/items/27bc5e9d7e93214c01f0) Japanese ver.
Or you can refer [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-connect/) English ver.

You need Azure login / download / import.

This operation is really important. It is not only for Azure CLI but also for Chef.


### 2.2. Provisioning the build server

If you want to build your app with non-windows server, you need to create a build server with VSO agent.

#### 2.2.1. Manual setting

You can setup a build server using this procedure. If you don't want to create by yourself, you can read this article,
then go 2.2.2.

[Parts Unlimited MRP : HOL - Parts Unlimited MRP App Continuous Integration with Visual Studio Online Build](https://github.com/Microsoft/PartsUnlimitedMRP/blob/master/docs/HOL_Continuous-Integration-with-Visual-Studio-Online-Build/HOL_Continuous-Integration-with-Visual-Studio-Online-Build.md)

#### 2.2.2. Using Chef Provision (Option)

If you don't want to create a build server by your self, you can use this chef recipe for VSO build agent.
Create a build server then set up the vso settings.

[Visual Studio Online Build Agent Machine for Ubuntu](https://github.com/TsuyoshiUshio/vsoagentserver)

### 2.3. Provisioning the production server

If you publish your software continuously, you need to set up the production server. Following this procedure.

#### 2.3.1. Provisioning the production server

This procedure will provision the production server using Azure SDK(xPlat). This article dosen't mention about Azure SDK subscription. Before doing this, please refer 2.1.3. and make sure to finsih azure login/download/import.

[Parts　Unlimited MRP :　HOL - Parts Unlimited MRP App Continuous Deployment with Visual Studio Online Build ](https://github.com/Microsoft/PartsUnlimitedMRP/blob/master/docs/HOL_Continuous-Deployment-with-Visual-Studio-Online-Build/HOL_Continuous-Deployment-with-Visual-Studio-Online-Build.md)

#### 2.3.2. Enable SSH login without password

To enalbe continuous deployment, you need to set up SSH login without password from the build server to the production server. Please log in the build server, then follow this procedure. (Please change the production server's name(e.g. puldev5.cloudapp.net)).

```
$ ssh-keygen -t rsa
$ cat .ssh/id_rsa.pub | ssh azureuser@puldev5.cloudapp.net 'cat >> .ssh/authorized_keys'
```

Then test it. If you can login the production server without being asked password then OK.

```
$ ssh azureuser@puldev5.cloudapp.net
puldev5 $ exit
```

You are ready. Just start VSO agent on the build server.

```
$ cd myagent
$ node agent/myagent
```

Then change index.html on the Visual Studio Online.
Build will automatically start for you.

After deployment has been finished, see this url on your browser.

```
http://<production_server_name>.cloudapp.net:9080/mrp
```


3. Additional information
-------------------------

### 3.1. .NET application

If you want to try .NET apprication, you can try PartsUnlimited. Both You can do it on Windows / Mac.

[PartsUnlimited : Getting Started](https://github.com/Microsoft/PartsUnlimited/blob/master/docs/GettingStarted.md)

### 3.2. FAQ

You can find FAQ of the PartsUnlimitedMRP and PartsUnlimited. If you ask a question to supporters, you need to add the entry to this FAQ. Please contribute to the community! You can write it in English or Japanese.

[DevOps hackathon FAQ](https://github.com/TsuyoshiUshio/DevOpsHackathon/wiki/FAQ)
