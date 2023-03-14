---
title: "[Linux] Tomcat [1] - Setting Up Tomcat"
date: 2023-03-14 22:38:00 +0900
categories: [linux, tomcat]
tags: [linux, tomcat, server]
img_path: 
---

# Tomcat

## Installing Tomcat Server

1. Go to the [Tomcat](http://tomcat.apache.org/download-10.cgi) website and download the Tomcat binary archive using the link provided in the web page. In this tutorial, we will install **Tomcat 10.1.7**:

    ```shell
    $ cd ~/archives
    $ wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.7/bin/apache-tomcat-10.1.7.tar.gz
    ```

    > Check the [Which version?](http://tomcat.apache.org/whichversion.html) page of Tomcat and take a note of the required Java version. For Tomcat 10.1.7, Java version 11 and later are supported.

2. Check the installed Java version using `java -version`. If the JDK version requirement is not met, install the required Java version:

    ```shell
    $ java -version
    openjdk version "1.8.0_362"
    $ yum install java-11-openjdk-devel.x86_64
    $ java -version
    openjdk version "11.0.18"
    ```

    > If the `java -version` still shows the old version, configure the system to use the latest Java version with the command `update-alternatives --config java`.

3. Extract the downloaded archive in step 1 to a directory. That directory will serve as the home of the Tomcat server:

    ```shell
    $ tar -xzvf apache-tomcat-10.1.7.tar.gz -C ~/tomcat
    ```

4. Go to the *bin* directory under the extracted Tomcat directory and run **startup.sh**. The script will print some path configurations and the message `Tomcat started.` when the server has been successfully set up:

    ```shell
    $ cd ~/tomcat
    $ cd apache-tomcat-10.1.7
    $ cd bin
    $ ./startup.sh
    Using CATALINA_BASE:    /home/tomcat/apache-tomcat-10.1.7
    Using CATALINA_HOME:    /home/tomcat/apache-tomcat-10.1.7
    Using CATALINA_TMPDIR:  /home/tomcat/apache-tomcat-10.1.7/temp
    Using JRE_HOME:         /usr/lib/jvm/java-11-openjdk-11.0.18.0.10-1.el7_9.x86_64
    Using CLASSPATH:
    Using CATALINA_OPTS:
    Tomcat started.
    ```

    > The path for JRE_HOME may show up empty. In that case, create the `setenv.sh` file in the *bin* directory and export the JRE_HOME variable with the correct path. The `setenv.sh` script will then be called by the `catalina.sh` script automatically on startup. For example:
    `export JRE_HOME=/usr/lib/jvm/java-11-openjdk-11.0.18.0.10-1.el7_9.x86_64`

5. Access the Tomcat server by navigating to the following address in a web browser:

    ```
    localhost:8080
    ```

6. If you are greeted by the Tomcat home page, then the installation is successful.

## Troubleshooting

TODO