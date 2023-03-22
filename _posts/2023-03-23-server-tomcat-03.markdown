---
title: "Tomcat [3] - OpenGrok"
date: 2023-03-23 00:07:00 +0900
categories: [Web, Web/Tomcat]
tags: [linux, tomcat, server, web, opengrok]
img_path: 
---

## OpenGrok

**OpenGrok** is a Java servlet application that indexes files and provides a convenient way to browse them. It is especially useful for viewing source files of a large project.

## Prerequisites

### OpenJDK 11
### Tomcat 10
### Universal Ctags

Install **Universal ctags** from source:

```shell
$ git clone https://github.com/universal-ctags/ctags.git
$ cd ctags
$ ./autogen
$ ./configure
$ make
$ sudo make install
```

> If *./autogen* ends with the message 'No autotools (autoconf and automake) found', install the required packages: `sudo yum install autoconf automake`

Make sure the installed **ctags** is the **Universal ctags**:

```shell
$ ctags --version
Universal Ctags 6.0.0 ....
```

## Downloading OpenGrok Binary

Download the OpenGrok binary archive:

```shell
$ wget https://github.com/oracle/opengrok/releases/download/1.9.2/opengrok-1.9.2.tar.gz
```

## Initializing OpenGrok Directory

Create an **OpenGrok** directory and its subdirectories:

```shell
$ mkdir -p opengrok/{src,data,dist,etc,log}
```

The resulting file structure should look like the following:

```shell
$ tree opengrok
opengrok
|-- data
|-- dist
|-- etc
|-- log
|-- src
```

Extract the downloaded **OpenGrok** binary to `opengrok/dist`:

```shell
$ tar -C ~/opengrok/dist --strip-components=1 -xzvf opengrok-1.9.2.tar.gz
```

> The `strip-components` option ensures that only the subdirectories of the opengrok-1.9.2 directory are extracted to the specified path

The resulting file structure now should look like this:

```shell
$ tree opengrok -L 2
opengrok
|-- data
|-- dist
|   |-- doc
|   |-- lib
|   |-- share
|   |-- tools
|-- etc
|-- log
|-- src
```

## Deploying OpenGrok in Tomcat

The `.war` file of each web application needs to be copied to the Tomcat **webapps** directory before it can be used. 

The `source.war` file of OpenGrok is located in `opengrok/dist/lib` directory.

```shell
$ cp ~/opengrok/dist/lib/source.war ~/tomcat/webapps
```

> The webapps can be accessed using their .war file name. In the case of OpenGrok, access it via Tomcat with the following link: `localhost:8080/source`

Go to `localhost:8080/source` in a web browser. The browser will show an error page saying that the configuration is missing. This is to be expected as no source files have been indexed yet.

At this point, Tomcat should have extracted the **source.war** file and created a **source** directory in the **webapp** folder.

## Indexing Files

First, copy the default **logging.properties** file to the `opengrok/etc` folder:

```shell
$ cp ~/opengrok/dist/doc/logging.properties ~/opengrok/etc
```

Copy the source files that need to be indexed to the `opengrok/src` folder. Make sure to create a parent folder for each project. A valid file structure will look as the following:

```shell
$ tree ~/opengrok/src
~/opengrok/src/
|-- sample_project/
    |-- hello.c
    |-- hello2.c
    ...
|-- sample_project2/
...
```

Then, run the indexer with the following command:

```shell
$ java \
-Djava.util.logging.config.file=~/opengrok/etc/logging.properties \
-jar ~/opengrok/dist/lib/opengrok.jar \
-c /usr/local/bin/ctags \
-s ~/opengrok/src -d ~/opengrok/data -H -P -S -G \
-W ~/opengrok/etc/configuration.xml -U http://localhost:8080/source
```

Browse the source files for each project in `localhost:8080/source`

## Troubleshooting

TODO