<!--
title: "Configuring Contrast as a Distributed Deployment"
description: "Instructions for configuring TeamServer in a distributed fashion by separating the application/container from the database."
tags: "EOP distributed configuration database scalability"
-->

## Who Should Read This Document
This guide is for Enterprise-On-Premises (EOP) administrators, who want to move away from the bundled, self-contained Contrast configuration in favor of a distributed configuration in which the database and application server are deployed on separate servers. Most customers will not have the need to configure a distributed TeamServer. Customers who do fit this profile are likely running with 100 or more connected agents and are seeking greater performance and scalability. 

## About Distributing The Contrast Configuration
EOP customers typically install and update the Contrast TeamServer by downloading the installer/updater artifact from the [Contrast Hub](https://hub.contrastsecurity.com). Instructions can be found [here](admin_tsinstall.html#install).

Some customers have the need to run a more advanced configuration. This document is intended for such customers, who require additional administration and management by an EOP administrator. We have simplified the configuration of TeamServer for EOP administrators to run their own installations of Tomcat, Java and MySQL, as long as they conform to our version requirements.

This documentation will guide you through the setup and configuration of additional software, but please be aware that you are responsible for the monitoring and durability of this software.  That being said, if you are familiar with installing and administering Tomcat and MySQL, the process is straightforward to set up and maintain.

Check back often for updates and as always, please feel free to submit a pull request if you have suggestions or find any instructions incorrect.   

## Before You Get Started
Before you get started with configuring a distributed TeamServer, make sure to read through the entire document. We've made several assumptions, which we list below. Make sure these conditions hold true so that you don't run into an issue with your TeamServer. The following assumptions have been made prior to distributing the configuration:

* Previous successful installation of Contrast EOP with a distributed database configuration
* Successful backup(s) and exports of the TeamServer database
* Collect Contrast TeamServer version numbers for the Application and Database 

In this example, Contrast has been installed at path `/usr/local/contrast`. To collect Contrast TeamServer application version numbers, look in the VERSION file in `/usr/local/contrast/VERSION`. To collect the Contrast TeamSever database version please run the following query: ``select `version` from schema_version ORDER BY `installed_on` DESC LIMIT 1;``. Let's say the app version stands at: `3.3.2` and the database is at `3.3.2.012`. We can say the versions are the same as it is safe to drop `.012` from the db version. Let's assume that you have an existing application server (A) running with a separate database (B), both running `3.3.2` and you're about to install `3.4.2` onto a new application server (C) and connect it to B. You will either need to stop A before installing `3.4.2` on C, or you will need to update A before installing on C.  

## Collect Configuration From Current TeamServer
In the example below, Contrast has been installed at path `/usr/local/contrast`.  You will need to gather the following:
* data/conf/
* data/esapi/
* data/.contrast
* data/.initialized
* data/contrast.lic

Compress these files into a zip file or a *tar.gz* file. Examples of Linux commands that will compress necessary artifacts into your user's home directory include:
```
$ cd /usr/local/contrast
$ tar -czvf ~/ctdc.tar.gz data/conf data/contrast.lic data/esapi/ data/.initialized data/.contrast
```

## Running The Installation
It is possible to run the installation as a regular user, however we recommend installation to your system as a *privileged* user.  This means if you are on Windows, you can right-click the installer and select **Run As Administrator** and if you are on Linux, use the ```sudo``` command to launch the installer.

Once you have launched the installer, you will be presented with several questions. Please select the "Application Server Only" installation option when prompted. Provide the compressed file you created in the previous section and follow the onscreen steps.

> **NOTE:** Pay particularly close attention to the value used for the TeamServer URL. This is the URL that client agents will use to communicate back to the TeamServer. We make our best attempt to determine the hostname and pre-populate this value, but if the provided hostname is not resolvable by clients on the network, they won't be able to communicate back to the server.

After the installation is complete, the TeamServer will perform its initial configuration and can take 2-3 minutes to fully start up. You can check the status of startup by watching `server.log` and `contrast.log`. Once the server has started successfully you will see something similar to the following in `server.log`:
```
260916 20.18.25,837 {} {} {} INFO  (Server.java:303) Contrast TeamServer Ready - Took 119305ms
```