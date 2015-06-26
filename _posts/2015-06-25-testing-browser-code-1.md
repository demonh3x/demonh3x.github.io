---
layout: post
title:  "Testing JavaScript code in the browser - part I"
date:   2015-06-25 19:20:00
categories: 8thlight
---
Testing user-interface code is hard. I don't know any tool that makes this easy.
The complication comes from the need of simulating what the user will do and asserting the results of that.
To be able to test a user-interface in a browser, we need tools to automate it.

These posts are based on what I've learned trying to test browser user-interfaces and the content may be very far from ideal.
I'm going to use a very simple example to guide these posts: a JavaScript counter widget.

I've described previously in [another blog post][js-ecosystem-2] some of the tools I'm going to use: Jasmine, PhantomJS and JQuery.
I also mentioned in that post one problem that we have to be aware of: Jasmine will not start from a clean state of PhantomJS each time it runs a test. Let's put an example:

[js-ecosystem-2]: /8thlight/2015/06/23/javascript-ecosystem-part-2.html

{% highlight javascript %}
function CounterWidget() {
  this.initialize = function() {
    $('body').append($('<div class="counter"/>'));
  }
}

describe('Counter widget', function() {
  it('does not create any instance before initializing', function() {
    var counter = new CounterWidget();
    expect($('.counter').size()).toEqual(0);
  });

  it('creates one instance when initializing', function() {
    var counter = new CounterWidget();
    counter.initialize();
    expect($('.counter').size()).toEqual(1);
  });
});
{% endhighlight %}

If we run that code, the tests will pass:

{% highlight html %}
Counter widget
✔ Counter widget does not create any instance before initializing
✔ Counter widget creates one instance when initializing

2 specs, 2 successes
Finished in 0.007 seconds
{% endhighlight %}

But if we change the order of the tests, executing `creates one instance when initializing` before `does not create any instance before initializing`. This will happen:

{% highlight html %}
Counter widget
✔ Counter widget creates one instance when initializing
✖ Counter widget does not create any instance before initializing

Failures:
Counter widget does not create any instance before initializing
Expected 1 to equal 0.

2 specs, 1 success, 1 failure
Finished in 0.011 seconds
{% endhighlight %}

When the test `does not create any instance before initializing` is executed, there is a counter in the DOM! Why is that?
I guess that's because Jasmine and PhantomJS are keeping the same DOM content during all the tests.
Colin Jones explains better than me [the consequences of having that in our tests][flaky-crusts].

[flaky-crusts]: http://blog.8thlight.com/colin-jones/2014/10/22/flaky-crusts-test-pollution.html

We need to do the cleaning of the DOM ourselves. To achieve that, we could add an `afterEach` step:

{% highlight javascript %}
afterEach(function () {
  $('.counter').remove();
});
{% endhighlight %}

With that, our tests are now independent and will pass.

But if the widget adds more elements that are not inside a `counter` element, those are not going to be cleaned. Also, we have another problem: we are always adding the counter to the `body`. And we may not want that to be decided by the widget.

There is an alternative that will give us better control and modularity:

{% highlight javascript %}
function CounterWidget(containerId) {
  this.initialize = function() {
    $('#' + containerId).append($('<div class="counter"/>'));
  }
}

describe('Counter widget', function() {
  beforeEach(function () {
    $('body').append($('<div id="test-container"/>'));
  });

  afterEach(function () {
    $('#test-container').remove();
  });

  it('creates one instance when initializing', function() {
    var counter = new CounterWidget('test-container');
    counter.initialize();
    expect($('.counter').size()).toEqual(1);
  });

  it('does not create any instance before initializing', function() {
    var counter = new CounterWidget('test-container');
    expect($('.counter').size()).toEqual(0);
  });
});
{% endhighlight %}

So far, I used JQuery for the testing side as well as the production side. But what if we don't want to use JQuery in our production code? A good outcome of passing the `containerId` directly as a `string` instead of anything related with JQuery is that we can change the implementation of the production code and keep our implementation of the tests working.

For example we could use plain old JavaScript DOM manipulation:

{% highlight javascript %}
function CounterWidget(containerId) {
  this.initialize = function() {
    var counter = document.createElement('div');
    counter.className = 'counter';
    document.getElementById(containerId).appendChild(counter);
  }
}
{% endhighlight %}

But I'm going to keep using JQuery to keep things simple.
