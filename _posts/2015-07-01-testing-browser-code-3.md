---
layout: post
title:  "Testing JavaScript code in the browser - part III"
date:   2015-07-01 19:00:00
categories: 8thlight
---
In [part II][p2] we [made the counter work][makeitwork]! The code we had was:

[p2]: /8thlight/2015/06/27/testing-browser-code-2.html
[makeitwork]: http://c2.com/cgi/wiki?MakeItWorkMakeItRightMakeItFast

{% highlight javascript %}
function CounterWidget(containerId) {
  var container = $('[data-id="' + containerId + '"]');
  var renderTemplate = function(count) {
    container.html(
      '<div data-counter>' +
        '<span data-count>' + count + '</span>' +
        '<input type="button" data-increment>' +
      '</div>'
    );
  };

  this.initialize = function() {
    var count = 0;
    container.on('click', '[data-increment]', function() {
      count++;
      renderTemplate(count);
    });
    renderTemplate(count);
  };
}
{% endhighlight %}

But there is an important concern here. We are testing the business logic through the user-interface. That is a [bad][scam-post] [idea][scam-video].

[scam-post]: http://blog.thecodewhisperer.com/2010/10/16/integrated-tests-are-a-scam/
[scam-video]: https://vimeo.com/80533536

It doesn't make sense that we need a Browser with JQuery and all the DOM stuff to test that a variable is incremented (sigh...). We want to be able to test the counting logic in isolation.

But right now, it is embedded in the widget code. The first thing we need to do is separate those responsibilities.

{% highlight javascript %}
function Counter() {
  var count = 0;

  this.increment = function() {
    count++;
  };

  this.getCount = function() {
    return count;
  };
}

function CounterWidget(containerId, counter) {
  var container = $('[data-id="' + containerId + '"]');
  var renderTemplate = function(count) {
    container.html(
      '<div data-counter>' +
        '<span data-count>' + count + '</span>' +
        '<input type="button" data-increment>' +
      '</div>'
    );
  };

  this.initialize = function() {
    container.on('click', '[data-increment]', function() {
      counter.increment();
      renderTemplate(counter.getCount());
    });
    renderTemplate(counter.getCount());
  };
}
{% endhighlight %}

Notice how all the counting details are now in the `Counter` and the `CounterWidget` receives the `counter` as a parameter.

As a consequence, we need to change the code that was creating the `CounterWidget` to this:

{% highlight javascript %}
var counter = new CounterWidget('test-container', new Counter());
{% endhighlight %}

This way we can now test and use the `Counter` without anything related to the `CounterWidget`.
And to test the `CounterWidget` in isolation we can inject [test doubles][littlemocker] in that parameter.

[littlemocker]: https://blog.8thlight.com/uncle-bob/2014/05/14/TheLittleMocker.html
