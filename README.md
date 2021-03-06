# SpringBoot MarkLogic Sample [![Build Status](https://travis-ci.org/nikos/springboot-marklogic-sample.svg?branch=master)](http://travis-ci.org/nikos/springboot-marklogic-sample)

UPDATE (2015-04-27): If you are interested in a more complex / real-world example, you might want to checkout the [WM14](https://github.com/jojrg/wm14) demo application. This application is built on the same technology stack and similar ideas, but uses player (plus tweets by the players) and match data from the Soccer Worldcup 2014 in Brazil to demonstrate integrating a Java middle-tier. 

## Goal

Build a simple (thin) web application with [Spring Boot](http://projects.spring.io/spring-boot/) to 
demonstrate how to access domain-specific data (for the purpose of this sample: products) 
from MarkLogic via its Java API. According to [Pivotals web site](https://spring.io/blog/2013/08/06/spring-boot-simplifying-spring-for-everyone)
> Spring Boot aims to make it easy to create Spring-powered, production-grade applications and services with minimum fuss.

To interact easily with the exposed REST endpoints there is a small AngularJS web client sitting on top.
For better understanding how MarkLogic's Java Client API handles JSON and XML documents
both formats are supported in the sample application.


## Motivation

Since [February 2013](http://www.marklogic.com/press-releases/marklogic-simplifies-development-of-enterprise-ready-applications-free-developer-license-for-marklogic-enterprise-edition-now-available/) 
everyone can get a [free MarkLogic Developer License](http://developer.marklogic.com/free-developer), 
which gives access to a powerful (= "Enterprise") NoSQL database and application platform, allowing
to store and index different kind of document types and search by various ways to quickly drill-down
to data you are looking for. Note: This sample does only touch the tip of the iceberg regarding
[MarkLogic's (search) features](http://www.marklogic.com/what-is-marklogic/enterprise-nosql/), 
it is really meant only to give you an idea how easy it is building applications with it.

As a Java developer I thought it was about time to start learning about MarkLogic server
and how to use the Java API to deal with JSON and XML documents in regards to creation,
binding and also query capabilites. With the recent advent of [Spring Boot](http://projects.spring.io/spring-boot/)
I wanted to show case how easy and straight forward it is, and how less Java code it requires,
to get a small (state-of-the-art with microservices, plus bells and whistles ready for production) 
web application up and running.


## Software Requirements

### MarkLogic Server

* [Download MarkLogic server](http://developer.marklogic.com/products/marklogic-server) (version 7), Please note: you need to create an account 
  with the MarkLogic developer community

* [Install, start and setup](http://docs.marklogic.com/guide/installation/procedures#id_28962) your MarkLogic server instance,
   
* Bootstraping the database and creating an associated REST API instance (that will run on port 8110, see ```gradle.properties```)
  is handled by the gradle plug-in [ml-gradle](https://github.com/rjrudin/marklogic-java/wiki/ml-gradle-Overview).
  This allows you to execute from the command-line ```gradle mlDeploy``` initially,
  the "deployment" also takes care of importing the required search options.


### Gradle

To compile and start the application you require a Java Development Kit (JDK 7) as well
as [Gradle](http://www.gradle.org/).


### Bower

For managing client-side dependencies (in this sample application: AngularJS and Bootstrap),
please install [bower](http://bower.io/) if you haven't already. This requires **Node.js** 
and **NPM**. To install both, the easiest is to follow the instructions on the **[Node.js homepage](http://nodejs.org)**.

    npm install -g bower

Then we need to run Bower (from this project's root directory) the first time to download client-side dependencies
in the proper directory:

    bower install

To adjust the target directory bower will save the downloaded dependencies please customize the file ```.bowerrc```.


## Try it out

First you need to adjust the configuration file which holds specifics about
how your MarkLogic server can be connected to, the easiest way is by copying
the file and modifying the connection string according to your settings:

    vi src/main/resources/application.yml

NOTE: This will be refactored to make use of spring profiles in a future version.

To give the sample web application a spin, check out the sources from github 
and start the application directly from the command-line by executing:

    gradle bootRun

If you want to open the sources with your favorite IDE, you might want to generate project files for Eclipse resp. IntelliJ IDEA with:

    gradle eclipse
    gradle idea

To start the app in debug mode (Port 5005 by default), run:

    gradle bootRun --debug-jvm


### Interact with the REST endpoints

The following examples use [httpie](http://httpie.org) as user-friendly cURL replacement.

#### Create a new product

    http POST localhost:8080/products sku=4711 name='Super Duper' description='with bars...'

Look out for the Location HTTP Header allowing to retrieve this entity to a later point in 
time again.

#### Retrieve a single product

    http GET localhost:8080/products/4711.json

#### Search for products contain a certain string

    http GET localhost:8080/products.json name=='Super Duper'

#### Delete the product

    http DELETE localhost:8080/products/4711.json


## Implementation details

### Spring Beans

The following diagram shows which Spring beans are used by the sample application:

![Spring Beans](https://raw.githubusercontent.com/nikos/springboot-marklogic-sample/master/doc/springbeans.png)


### Enable auto-refresh:

By using spring-loaded (see also https://github.com/spring-projects/spring-boot/issues/887)
it is possible (to some degree) to exchange recompiled classes while 
your application stays up running.

Set it up under VM options in your IDE (make auto-compile after save is working):

    -javaagent:/path/to/springloaded-1.2.0.RELEASE.jar -noverify


## Further reading

* [Introduction to the MarkLogic Java API](https://docs.marklogic.com/guide/java/intro)
* [Spring Boot Reference](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
* [Spring Actuator: exposing metrics and allow to monitor the application easily](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready)

<!--
   [Freemarker: template engine](http://freemarker.org/)
-->


## Feedback

In case of any questions or suggestions please get into 
[contact with the author](mailto:niko[at]nava[dot]de)
of course reporting issues or even contribute pull requests
via github are highly welcome.
