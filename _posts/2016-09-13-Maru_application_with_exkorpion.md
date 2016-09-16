---
layout: post
title:  "Testing your Rest API with Exkorpion"
date:   2016-09-13 11:15:02
description: Exkorpion.
categories: exkorpion elixir mix maru
banner_image: "posts/getting_started_with_elixir/elixir-logo.png"
comments: true
---

## Purpose

Along this guide we'll learn how to implement our tests with Exkorpion.  

## The environment

To follow this guide I assume you already have your **Elixir** development environment ready, besides you should have installed **docker** on our machine since
we need to run a postgresql database and we will be taking advantage of docker.

This guide will go over  **Maru** and **Ecto** but being focused on understanding **Exkorpion**. 

### Ecto

Ecto is a database wrapper that makes the life easier for Elixir developers who need to make use of database in their applications. 

To get a better understanding of Ecto, have a look at **[Getting started with Ecto](http://www.wesovilabs.com/exto/elixir/mix/2016/07/16/Working_with_Ecto/)**

### Maru

It's a REST-like API micro-framework for elixir inspired by grape. 

I encourage you to have a look at my previous article **[Getting started with Maru](http://www.wesovilabs.com/exto/elixir/mix/maru/2016/07/16/Elixir_maru_training/)**  

### Exkorpion

Exkorpion is test library that wraps ExUnit and enhances their features providing our tests with a **BDD syntax**. 

#### Goals

- It helps us to write tests which are easy-to-read.
- It is completely compatible with ExUnit.
- It forces us to structure our tests in steps (given-when-then)
- It is not coupled to any other framework

#### Syntax

As was mentioned on the above Exkorpion is mainly oriented to a bdd syntax:

**scenario**:  A scenario groups multiple cases that test a functionality works as expected. By using scenario we achieve the below:

  - Better documentation for other developers.
  - Test are better organized and structured
  - Working under an agile methodology we can match scenarios to acceptance criteria

**it**: Exkorpion provide with a reserved word It to represent any of the cases inside a scenario.

**before_each**: Before each will be inside of a scenario and provices with a reusable set of data for our tests.

**with/given/when/then**: These word are the ones that provide us with s BDD syntax. Actually even when we write some unit tests we should thinkg about them.

  - *Given*: It defines the input data for performing the tests. (It's an optional step, it could be not neccessary sometimes)
  - *When*:  It performs the action to be tested.
  - *Then*:  It ensures the result in the preoviuos step are the expected.


we could make us of **with** step if we pretend to run the some tests for multiple input

## Let's do it

This step-by-step guide will make use of a existing project which is implemented with **Maru** and **Ecto**. This project
 implement a versioned API, which offers the below services:
   
   | Version  |  Verb  |  Path        | Description |
   | ---      | ---    |  ---         |  --- |
   |v1        |  GET   |  /v1/tracks  |   Return the list of mocked tracks (It always returns two tracks) |
   |v1        |  POST  |  /v1/tracks  |   Add a new track and return the list of existing tracks (It will return 3 tracks) |
   |v2        |  GET   |  /v2/tracks  |   Return the list of tracks in the database |
   |v2        |  POST  |  /v2/tracks  |   Add a new track and return the list of existing tracks |
   {:.mbtablestyle}
   
The above records represent the services.

**Fork the repository**: Go to Github and fork this repository: **[elixir_maru_training](https://github.com/wesovilabs/elixir_maru_training)** onto your own
workspace.

![Fork](https://raw.githubusercontent.com/wesovilabs/wesovilabs.github.io/master/assets/images/posts/exkorpion/project-forked.png)

**Clone your repository**

  {% highlight bash %}
    git clone https://github.com/ivancorrales/elixir_maru_training.git
    cd elixir_maru_training
    mix deps.get   
  {% endhighlight %}


## The code

