---
layout: post
title:  "[Draft] A better way to run our acceptance tests."
date:   2015-04-18 11:15:02
description: Acceptance Tests with Spring Boot and Docker
categories:
- DevOps
- Micro services
- Acceptance tests
- Integration tests
- Cucumber
- Spring boot
- Rest Assured
- Docker
- Docker-compose
permalink: docker-spring-boot
banner_image: "posts/cucumber/banner.jpg"
comments: true
---

## Introduction

Since I assume you already know Spring Boot and the benefits of this one,this example will be exclusively focused on showing how to write acceptance tests agains a virtual environment that runs on docker containers and they are orchestrated by compose (previously known as flig).

# Scenario

The example will consist on building a couple of micro services: One will persist data into the data base and the second one will take this information from the database. To sum up we will get 3 containers (1 per each micro service and the third for the database) which are represented on the below:

![Containers](http://localhost:4000/assets/images/posts/001/containers.png#img-responsive)

## Purpose

The purpose of this article is run our acceptance tests for a couple of micro services which are installed on docker containers, and being orchestrated by Compose (previously known as flig).


## Let's do it

First of all, we will create our Spring micro services. We can do it taking advantage of this site [https://start.spring.io/](https://start.spring.io/) which I found recently and It is so handy since it provides us with a skeleton of a spring boot project.

You can find the code on the following [link](http://www.wesovi.com/microservice-docker-compose/)

If you download the code you will see a maven project with three submodules:

  * **ms.insertion**: Micro service I : Insert items into the database.
  * **ms.extraction**: Micro service II : Extract items from the database.
  * **ms.at**: Acceptance Tests project.

Each micro service module contains a couple of integration tests which you can run by the following command: **mvn integration-test -P integration**


Now, that we already have our two micro services correctly implemented, we will create our acceptance tests making use of cucumber. Below we can see the feature

{% highlight ruby %}
Feature: Micro Services Acceptance Tests

Scenario: Getting the list of items when the database is empty
  Given a database with no items
  When we ask for the list of items
  Then we obtain no items

Scenario: Getting the list of items when the database already contains items.
  Given the items database is initialized with the following data
    | name 	| description		|
    | item001	| Bargains		|
    | item002	| Promotions	        |
    | item003	| Exclusive		|
    | item004	| Out of date	        |
  When we ask for the list of items
  Then we obtain '4' items

Scenario: We add new items to an empty database
  Given a database with no items
  When we add the below item
    | name    | description |
    | item001 | Bargains    |
  And we ask for the list of items
  Then we obtain '1' item

Scenario: We add new items to an empty database
  Given the items database is initilized with the folloing data
    | name 	| description		|
    | item001	| Bargains		|
    | item002	| Promotions		|
    | item003	| Exclusive		|
  When we add the below item
    | name 	 | description	|
    | item004	 | Out of date  |
  And we ask for the list of items
  Then we obtain '4' item
{% endhighlight %}

Until few time ago we would run the acceptance tests by following the below steps:

  *   Launching the web server
  *   Deploying the war onto the application server.
  *   Running our acceptance tests agains
  *   Stopping the application server.

The above way to run our acceptance tests is fine but not great since we would be running our acceptance tests in an environment completely different to production.

In the last year many people are betting for working with containers to simulate a production environment, so please do not forget about testing and take advantage of this to move another step forward.

## Conclusions
Below some pros and cons of doing acceptance tests as is explained in this article:

  *   **Pros**
  *   Testing in a real environment.
  *   Adapting our Tests to the new way to do things.
  *   Improving our continuous integration.
  *   Adding quality to our developments.
  *   Introduce developers to a DevOps world.


  * **Cons**
  *   This could take  bit longer than running our tests in our local environment.
  *   Developers need to have basic knowledge about docker.

## Code
You can find the code on my github repo [link](http://www.wesovi.com/microservice-docker-compose/)

The steps to run the example are explained on Git repository (README.md), Be aware of the first time you run the example this will take a little more as the virtual environment is created. In this example, I assume you have vegrant installed on your computer.

I am still working on this example but in the meantime I encourage you to fork my project and make some pull requests!

## Referenes
Useful links:

  *   [Great post about Integration Testing in Spring Boot](http://g00glen00b.be/spring-boot-rest-assured/)


  *   [First steps with Cucumber](http://www.hascode.com/2014/12/bdd-testing-with-cucumber-java-and-junit/)

___
