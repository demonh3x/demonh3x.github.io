---
layout: post
title:  "Higher order functions - step by step - part III"
date:   2015-07-10 18:00:00
categories: 8thlight
---
Return functions
----------------

Let's imagine we want to `filter` the `people` in different ways. Not only the ones who `canDrive`. Let's suppose we also want to `filter` the `people` who are named "Jane".

{% highlight javascript %}
var drivers = filter(people, canDrive);

var isNamedJane = function(person){
  return person.name == "Jane";
};
var janes = filter(people, isNamedJane);
{% endhighlight %}

We are passing the `people` to each one of those calls. If we were only filtering `people`, that may seem redundant. We could define a function that only filters on the `people`.

{% highlight javascript %}
var filterPeople = function(predicate){
  return filter(people, predicate);
};

var drivers = filterPeople(canDrive);
var janes = filterPeople(isNamedJane);
{% endhighlight %}

But that only works in this context because it is tied directly to `people`. We would like to do that with any collection.

{% highlight javascript %}
var filterCollection = function(collection){
  return function(predicate){
    return filter(collection, predicate);
  };
};
var filterPeople = filterCollection(people);

var drivers = filterPeople(canDrive);
var janes = filterPeople(isNamedJane);
{% endhighlight %}

The result of the function `filterCollection` is another function with one parameter less. That is called [partial application][partial]. Which means that we are predefining arguments.

[partial]: http://en.wikipedia.org/wiki/Partial_application

But that is only valid for the `filter` function and only for the first argument. We would like to do that with any function and any amount of arguments.

{% highlight javascript %}
var bind = function(initialFunction, argument1, argument2, argumentN){
  var partialArguments = Array.prototype.slice.call(arguments, 1);
  return function(){
    var remainingArguments = Array.prototype.slice.call(arguments);
    return initialFunction.apply(null, partialArguments.concat(remainingArguments));
  };
};
var filterPeople = bind(filter, people);

var drivers = filterPeople(canDrive);
var janes = filterPeople(isNamedJane);
{% endhighlight %}

Now we can only `bind` the first arguments to a function. We would like to `bind` any combination of arguments to a function. But this time, intead of trying to generalise `bind` for that purpose, we are going to `cycle` the arguments of a function so we can then `bind` the ones we prefer.

{% highlight javascript %}
var cycle = function(initialFunction) {
  return function(argument1, argument2, argumentN){
    var args = Array.prototype.slice.call(arguments);
    args.push(args.shift());
    return initialFunction.apply(null, args);
  };
};
var filterDrivers = bind(cycle(filter), canDrive);
var filterJanes = bind(cycle(filter), isNamedJane);

var drivers = filterDrivers(people);
var janes = filterJanes(people);
{% endhighlight %}

The last thing we would like to do is to `compose` a single function from other functions, having the output from one function going to the input of the next. With that, we can reproduce the behaviour of the first piece of code in the starting point.

{% highlight javascript %}
var compose = function(function1, function2, functionN) {
  var functions = arguments;
  return function(input) {
    var output = input;
    var i;
    for (i = 0; i < functions.length; i++) {
      output = functions[i](output);
    }
    return output;
  };
};

var onlyDrivers = bind(cycle(filter), canDrive);
var getTheirNames = bind(cycle(map), getName);
var printThem = bind(cycle(each), consoleStream.write);

compose(onlyDrivers, getTheirNames, printThem)(people);
{% endhighlight %}

There we have code at a much higher level of abstraction. In contrast with the code at the starting point, this one enables us to see what the program is doing in human terms: given `people` treats `only drivers`, `gets their names` and `prints them`.

Each function does only one thing and does it well. You can extend their functionality by combining them, not modifying them. Because they are functions, as long as you keep the signature, you can change their implementations and the users will not notice. All of that makes them very loosely coupled and as a result they are extremely easy to test as units.

You can feel the similarity with a SOLID-conforming object-oriented system and their very nice composable properties. I like to see [higher-order functions][hof] as a way to create a kind of lego system, a system with very flexible little pieces that make easy to create big and very expressive worlds.

![lego](https://camo.githubusercontent.com/4f535ad2ef5710b1242bfb262f993d5dbb87f722/687474703a2f2f63646e2e636f6c6c696465722e636f6d2f77702d636f6e74656e742f75706c6f6164732f6c65676f2d726976656e64656c6c2d6c6f72642d6f662d7468652d72696e67732d322e6a7067)
