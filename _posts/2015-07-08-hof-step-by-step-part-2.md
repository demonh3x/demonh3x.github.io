---
layout: post
title:  "Higher order functions - step by step - part II"
date:   2015-07-08 21:00:00
categories: 8thlight
---
Receive functions as arguments
------------------------------

To swap the code that we execute on `each` element of a collection we previously introduced the `stream` objects. That way we had a common way to use the options we had at the moment. If we want to make it more generic, we need to remove more specific details from that. You can think that someone receives a `text` and the outcome of that call is something that cannot be seen by the caller. That way we have removed the `stream` and `write` concepts. What we have left is just a function that receives a `text` and does not return anything.

{% highlight javascript %}
var each = function(collection, elementHandler) {
  var i;
  for (i = 0; i < collection.length; i++) {
    elementHandler(collection[i]);
  }
};

//in some part of the code:
each(eligibleDriverNames, consoleStream.write);

//in other part of the code:
each(eligibleDriverNames, fileStream.write);
{% endhighlight %}

Now, this `each` function is very generic. It can be reused in a very wide context.

We can do the same with the remaining piece of code from the beginning:

{% highlight javascript %}
var eligibleDriverNames = [];
for (var i = 0; i < people.length; i++) {
  if (people[i].age >= minimumAgeToDrive) {
    eligibleDriverNames.push(people[i].name);
  }
}
{% endhighlight %}

To be able to do that we need to separate the parts involved in [each responsibility][srp]. Let's generalise the one that `filters` the drivers first:

[srp]: http://en.wikipedia.org/wiki/Single_responsibility_principle

{% highlight javascript %}
var eligibleDriverNames = [];
for (var i = 0; i < people.length; i++) {
  if (people[i].age >= minimumAgeToDrive) {
    eligibleDriverNames.push(people[i]);
  }
}
{% endhighlight %}

{% highlight javascript %}
var filter = function(collection, predicate) {
  var filtered = [];
  var i;
  for (i = 0; i < collection.length; i++) {
    if (predicate(collection[i])) {
      filtered.push(collection[i]);
    }
  }
  return filtered;
};

var canDrive = function(person){
  return person.age >= minimumAgeToDrive;
};
var drivers = filter(people, canDrive);
{% endhighlight %}

And now the one that `maps` each driver to his/her name:

{% highlight javascript %}
var eligibleDriverNames = [];
for (var i = 0; i < drivers.length; i++) {
  eligibleDriverNames.push(drivers[i].name);
}
{% endhighlight %}

{% highlight javascript %}
var map = function(collection, transformation){
  var mapped = [];
  var i;
  for (i = 0; i < collection.length; i++) {
    mapped.push(transformation(collection[i]));
  }
  return mapped;
}

var getName = function(person){
  return person.name;
};
var eligibleDriverNames = map(drivers, getName);
{% endhighlight %}

By hiding the `for` loop inside these functions, we have created an [abstraction][abstraction]. We have separated *what to do* from *how to do it*. Now the clients can depend only on *what to do*. That enables the possibility to change *how to do it* without affecting them.

[abstraction]: http://en.wikipedia.org/wiki/Abstraction_(computer_science)

For example: In the `each` function *what we do* is: *do something with each element in the collection*. *How we do it* is: *iterating sequentially with the `for` loop*.

We could change the implementation of `each` for one that process the elements in parallel to take advantage of multi-core or several clusters. We could also change the implementation of `map` for one that does the transformation only if the result is going to be used ([lazy evaluation][lazy]).

[lazy]: http://en.wikipedia.org/wiki/Lazy_evaluation
