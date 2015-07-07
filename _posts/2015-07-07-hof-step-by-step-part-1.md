---
layout: post
title:  "Higher order functions - step by step - part I"
date:   2015-07-07 19:30:00
categories: 8thlight
---
Having the possibility to use functions as values in a programming language enables us to make [higher-order functions][hof]. That's a fancy way of saying that **a function receives and/or returns functions**.
That may seem simple but has tremendous implications. We are going to explore some of them here.

[hof]: http://en.wikipedia.org/wiki/Higher-order_function

To get better understanding of the mechanics, we are going to reimplement some [higher-order functions][hof] and that will give us an intuition of what may happen under the covers.

Starting point
--------------

{% highlight javascript %}
var minimumAgeToDrive = 17;
var people = [
  {name: "John", age: 17},
  {name: "Jane", age: 15},
];

var i;
for (i = 0; i < people.length; i++) {
  if (people[i].age >= minimumAgeToDrive) {
    console.log(people[i].name);
  }
}
{% endhighlight %}

This `for` loop, although small and harmless looking, contains highly entangled code. It can only be tested as a whole. It is full of specific details. Nothing in it can be reused. Users are going to be forced to duplicate the loop and most of the operations if they want to do something similar.

For example: let's suppose in some parts we want to print the names of the people who can drive. And, in other parts we want to save them in a file. We want to reuse everything but the process of doing something with the names of each eligible driver. What do we do?

{% highlight javascript %}
var eligibleDriverNames = [];
var i;
for (i = 0; i < people.length; i++) {
  if (people[i].age >= minimumAgeToDrive) {
    eligibleDriverNames.push(people[i].name);
  }
}

//in some part of the code:
var i;
for (i = 0; i < eligibleDriverNames.length; i++) {
  console.log(eligibleDriverNames[i]);
}

//in other part of the code:
var i;
for (i = 0; i < eligibleDriverNames.length; i++) {
  fs.appendFileSync('file', eligibleDriverNames[i]);
}
{% endhighlight %}

We have here structural duplication. We are repeating the same loop structure each time we do something with each element of a collection. The only difference between the last two loops is the place where we write the `name`. Because the code in those have completelly different APIs we need to introduce a common public interface to treat them as *similar things* in the sense that they can `write` a `text`. In an object oriented language we could do that like this:

{% highlight javascript %}
var consoleStream = {
write: function(text) {
         console.log(text);
       }
};
var fileStream = {
write: function(text) {
         fs.appendFileSync('file', text);
       }
};

var writeEligibleDriverNames = function(stream) {
  var i;
  for (i = 0; i < eligibleDriverNames.length; i++) {
    stream.write(eligibleDriverNames[i]);
  }
};

//in some part of the code:
writeEligibleDriverNames(consoleStream);

//in other part of the code:
writeEligibleDriverNames(fileStream);
{% endhighlight %}

But that will solve a part of the problem and only for this specific scenario. We don't want to be tied to `eligibleDriverNames` so we can reuse it in more scenarios. We could generalise it a bit more by passing it as a parameter like this:

{% highlight javascript %}
var writeEach = function(collection, stream) {
  var i;
  for (i = 0; i < collection.length; i++) {
    stream.write(collection[i]);
  }
};

//in some part of the code:
writeEach(eligibleDriverNames, consoleStream);

//in other part of the code:
writeEach(eligibleDriverNames, fileStream);
{% endhighlight %}

That code now is more generic but it is completely tied to the `write` method on the `stream` object. We want a more generic solution, one that we can reuse in a wider context. We need to be able to do anything with each element of the collection, not only `write` on a `stream`.
