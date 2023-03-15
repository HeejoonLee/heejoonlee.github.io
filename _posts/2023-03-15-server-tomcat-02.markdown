---
title: "Tomcat [2] - Accessing the Manager Web Application"
date: 2023-03-15 23:07:00 +0900
categories: [Web, Web/Tomcat]
tags: [linux, tomcat, server, web]
img_path: 
---

## Accessing the Manager Webapp

Bare Tomcat installation includes some basic web applications to help with the site management. The `manager webapp` will allow us to check the server status, deploy *.war* files and do other management activities via the web interface.

After the initial installation, the two buttons `Server Status` and `Manager App` are displayed on the top right side of the Tomcat home page. However, clicking any of the two buttons will lead us to the **403 Access Denied** page, denying us the right to use the `manager webapp`.

> The page displayed when *Server Status* is clicked may be different when accessing the Tomcat from the localhost: `localhost:8080`. If the proper user has not been set up, it will lead us to the **401 Unauthorized** page.

Tomcat includes some security measures to prevent anyone from easily accessing the `manager webapp`. To actually use the application, we have to enable a user with the role *manager-gui*.

## Creating a User with the *manager_gui* Role

Open the `tomcat-users.xml` file located in the `conf` directory in your base Tomcat installation folder:

```shell
$ cd ~/tomcat/apache-tomcat-10.1.7/conf
$ vim tomcat-users.xml
```

Add a user entry with the role **manager-gui** as the following:

```xml
<tomcat-users ...>
    ...
    <user username="tomcat" password="tomcat" roles="manager-gui"/>
    ...
</tomcat-users>
```

> The *username* and *password* fields are used to gain access to the *Manager Webapp*. They can be set arbitrarily. However, make sure the *roles* field is set to **manager-gui**.

Save the `tomcat-users.xml` file and restart Tomcat:

```shell
$ cd ~/tomcat/apache-tomcat-10.1.7/bin
$ ./startup.sh
```

## Gain Access to the Manager Webapp

Open a Web browser and browse to the Tomcat homepage:

```shell
localhost:8080
```

Click *Server Status* button on top.

When the browser asks for credentials, log in using the username and password set earlier.

The `manager webapp` is now accessible.

## Troubleshooting

TODO