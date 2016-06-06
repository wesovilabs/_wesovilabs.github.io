---
layout: post
title:  "First steps with Erlang"
date:   2016-01-29 00:00:00
categories: erlang demo tutorial example sample
banner_image: "posts/first-steps-with-erlang/Erlang.jpg"
comments: true
---

## Setting up the environment

Since I am working with a Mac, first step I will take is running the following command:  

        brew install erlang


## Purpose of this post

Just sharing with other people my opinion about my brief experience with Erlang and hopefully will give other 
people some tips on how to start coding in this language.


## Coding a basic program

Below the code which is hosted on Github

<script src="https://gist.github.com/ivancorrales/289dfe886ede6d9ec9ac.js"></script>

## Compiling the program

The above code must be compiled  so we’ll run the below command

    erlc <filename.erl>

and this will create a new file with the same name but with extension ***.beam**

## Running the program

Once we compile the code we can run the program with the following command: **erl -run poc all -run init stop -noshell**

This is not a hello world program at all but not a very complicated one neither. We got a list of people (men and women)
and after running the program we will only display by console a greeting to those people who are older than the average
of people in the list. In case of we greeting to a woman we’ll say “Hello miss…” and on the other hand we will say
“hello mister …” when greeting to a man.

To sum up the program execution will display the below:

    The average age is 31 years old.
    Hello miss "Mariah" , you're 51 and you are older than average people
    Hello mister "John" , you're 33 and you are older than average people
    Hello mister "Mick" , you're 42 and you are older than average people


## Understanding the code

Since I am not an experienced  functional-programmer I try to to understand it comparing with other programming languages which I got more experience working with.

Apologize in advance for all those mistakes I will be making in my explanation.

* **Module declaration**: This looks like the package declaration (or namespace) in other programming languages.
{% highlight erlang %}
-module(poc).
{% endhighlight %}

* **Declaring “public/accesible methods”**: By this we can state what methods/functions we could be run by command line ( when we ran the program we indicated the method “all”) Otherwise we need to state the number of arguments in the functions,
this looks weird from someone who comes from languages such as Java or Javascript..
{% highlight erlang %}
-export([all/0,say_hello/1]).
{% endhighlight %}

* **Record declaration**: I am sure any Java developer would understand this like a POJO, I mean looks like If we are declaring the object structure.
{% highlight erlang %}
-record(person,{name,genre,age}).
{% endhighlight %}

* **"Records constructor”**: - This is like when declaring a constructor in other programming languages.
{% highlight erlang %}
new_person(Name, Genre, Age) ->
  #person{name=Name, genre=Genre, age=Age}.
{% endhighlight %}

* **Declaring methods / functions**: I really like the idea of last line the code in a method is the response. This happens in Go or Scala (or both, don’t remember now…).
word when function returning a value
{% highlight erlang %}
isOlderThanAveragePeople(Age,AverageAge)->
  Age>=AverageAge.
{% endhighlight %}

* **Case statement**: Like in other programming languages, we as make use of this statement structure.
{% highlight erlang %}
say_hello(Person)->
  case Person#person.genre of
    "m" ->
      io:fwrite("Hello mister ~p , you're ~w and you are older than average people\n", [Person#person.name, Person#person.age]);
    "f" ->
      io:fwrite("Hello miss ~p , you're ~w and you are older than average people\n", [Person#person.name, Person#person.age])
  end.
{% endhighlight %}


* **Objects list creation**: Very similar to other programming languages, isn't it?
{% highlight erlang %}
all() ->

  John = new_person("John","m",33),
  David = new_person("David","m",18),
  Mick = new_person("Mick","m",42),
  Mariah = new_person("Mariah","f",51),
  Sarah = new_person("Sarah","f",11),
  People = [John,Mariah,David,Mick,Sarah],
  ...
{% endhighlight %}


* **Calling other methods/functions**: Nothing to explain about it...
{% highlight erlang %}
all() ->
    ...
    AverageAge = round(calculate_total_age(People) / length(People)),
{% endhighlight %}  


* **Iterating over the list:**: From my point of view, this is the only part which I’ve coded with a more functional-programming orientation
and probably the part of code which I've enjoyed coding much more… As we can see, we split the method declaration in three steps:
the first one assumes we receive a list of elements, the second one that we are retrieving a single element in the list, and the last one
will be use to process empty lists
{% highlight erlang %}
iterate([Person],AverageAge) ->
  case isOlderThanAveragePeople(Person#person.age,AverageAge) of
    true -> say_hello(Person);
    _ ->ok
  end;

iterate([Person|People],AverageAge) ->
  case isOlderThanAveragePeople(Person#person.age,AverageAge) of
    true -> say_hello(Person);
    _ ->ok
  end,
  iterate(People,AverageAge);

iterate([],_) ->
  ok.
{% endhighlight %}  


## Code
Code can be found on Github, [click here to download it](https://gist.github.com/ivancorrales/289dfe886ede6d9ec9ac#file-first-steps-with-erlang-erl)
