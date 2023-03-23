---
title: "Tomcat [4] - Authenticate Access to Webapps"
date: 2023-03-23 20:36:00 +0900
categories: [Web, Web/Tomcat]
tags: [linux, tomcat, server, web, opengrok, authentication]
img_path: 
---

In the previous post, we set up the **OpenGrok** webapp in Tomcat. All we had to do to use the webapp was to type the address `localhost:8080/source` in a web browser. 

The current method of usage is simple and convenient, but it may raise some security issues when the need arises to share the sources with other people.

There are many authentication methods that **Tomcat** provides to limit access to specific webapps. In this post, we will show you how to implement the *BASIC* authentication on **OpenGrok** webapp.

## Enabling the BASIC Authentication in OpenGrok

Each webapp hosted on **Tomcat** has a directory in the `tomcat/webapps` directory. If you followed the previous post to set up **OpenGrok**, there will be a `source` folder in the above directory.

We need to look for the `web.xml` file in the `WEB-INF` directory of the `source` folder:

```shell
$ cd ~/tomcat/webapps
$ vim source/WEB-INF/web.xml
```

> When opening the `web.xml` file with **Vim**, the screen may show no text at all. This is most likely due to file permission issues. Use **sudo** to open the file in that case.

Navigate to the bottom of the `web.xml` file. We will be adding some elements here to enable authentication in **OpenGrok**.

We first need to add a **login-config** element indicating the type of authentication to use:

```xml
<web-app>
    ...

    <login-config>
        <auth-method>BASIC</auth-method>
    </login-config>

</web-app>
```

We then need to add two **security-constraint** elements. 

One is for the `/api/*` url:

```xml
<web-app>
    ...
    </login-config>

    <security-constraint>
        <web-resource-collection>
            <web-resource-name>APIs</web-resource-name>
            <url-pattern>/api/*</url-pattern>
        </web-resource-collection>
    </security-constraint>

</web-app>
```

The second **security-constraint** is for the actual source files:

```xml
<web-app>
    ...
    </security-constraint>

    <security-constraint>
        <web-resource-collection>
            <web-resource-name>Protected source files</web-resource-name>
            <url-pattern>/*</url-pattern>
            <http-method>GET</http-method>
            <http-method>POST</http-method>
        </web-resource-collection>
        <auth-constraint>
            <role-name>opengrok_user</role-name>
        </auth-constraint>
    </security-constraint>

</web-app>
```

The first **security-constraint** is only applied to **OpenGrok** APIs. We set *no authentication constraint* on the APIs, so we would not run into any permission problems when we are indexing the source files.

The second **security-constraint** has the **auth-constraint** element set, with the role-name *opengrok_user*. This allows only users with the role *opengrok_user* to access the files. Notice that the authentication is applied for http method *GET* and *POST*, effectively blocking anyone without access from viewing the files.

## Adding Users with the *opengrok_user* Roles in Tomcat

We now need to add some users with the role-name we defined for **OpenGrok** authentication. 

Open the `tomcat-users.xml` file:

```shell
$ vim ~/tomcat/conf/tomcat-users.xml
```

Define the role *opengrok_user* and add a user with that role:

```xml
<tomcat-users>
    ...
    <role rolename="opengrok_user"/>
    <user username="opengrok" password="opengrok" roles="opengrok_user"/>

</tomcat-users>
```

Take note of the username and password just added. These will be used to gain access to the **OpenGrok** webapp.

Restart **Tomcat**:

```shell
$ cd ~/tomcat/bin
$ ./shutdown.sh
$ ./startup.sh
```

## Logging in with the Registered Credentials

Go to `localhost:8080/source`. The web page will now ask for a username and password before granting access. Enter the username and password that was added in the *tomcat-users.xml* file.

> The web browser may not ask for credentials but just display the error page. Try using a *private mode* to access the page instead.

## Troubleshooting

TODO