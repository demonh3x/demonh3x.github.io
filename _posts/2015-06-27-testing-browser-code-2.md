---
layout: post
title:  "Testing JavaScript code in the browser - part II"
date:   2015-06-27 12:30:00
categories: 8thlight
---
In [part I][p1] we proposed an interesting alternative that creates the widget in a `test-container` so we can clean properly the DOM after each test.

[p1]: /8thlight/2015/06/25/testing-browser-code-1.html

Before going on, I need to tell you that I've changed `class` and `id` attributes to `[data-*]`. [There are good reasons for it.][data-attrs]

[data-attrs]: http://stackoverflow.com/questions/5032841/html5-custom-attributes-why-would-i-use-them

Now, we can start adding some interesting behavior to the counter. To keep things simple we should start counting on 0. So, lets add the test:

{% highlight javascript %}
it('starts at 0', function() {
  var counter = new CounterWidget('test-container');
  counter.initialize();
  expect($('[data-count]').text()).toEqual('0');
});
{% endhighlight %}

To make that test pass we only need to add the `count` DOM element with `0` inside it:

{% highlight javascript %}
function CounterWidget(containerId) {
  var container = $('[data-id="' + containerId + '"]');
  var template = $(
    '<div data-counter>' +
      '<span data-count>0</span>' +
    '</div>'
  );

  this.initialize = function() {
    container.html(template);
  };
}
{% endhighlight %}

That was easy. The next simplest thing could be to count only one click. This will be the test:

{% highlight javascript %}
it('counts 1 click', function() {
  var counter = new CounterWidget('test-container');
  counter.initialize();
  $('[data-increment]').click();
  expect($('[data-count]').text()).toEqual('1');
});
{% endhighlight %}

Things start to be interesting here. To make that work, we have to do a couple of things: We have to capture the click event on the `data-increment`, and change the `data-count` to 1 when that happens.
We could do that like this:

{% highlight javascript %}
function CounterWidget(containerId) {
  var container = $('[data-id="' + containerId + '"]');
  var renderTemplate = function(count) {
    return $(
      '<div data-counter>' +
        '<span data-count>' + count + '</span>' +
        '<input type="button" data-increment>' +
      '</div>'
    );
  };

  this.initialize = function() {
    var template = renderTemplate(0);
    template.find('[data-increment]').click(function(){
      container.html(renderTemplate(1));
    });
    container.html(template);
  };
}
{% endhighlight %}

There's one thing to keep in mind: when we render a template, it doesn't have the event listeners attached. But this code works because we only care about the first click for now.

We almost have it, we need to keep adding 1 each time the user clicks on `data-increment`. So lets express that in the simplest test to do next:

{% highlight javascript %}
it('counts 2 clicks', function() {
  var counter = new CounterWidget('test-container');
  counter.initialize();
  $('[data-increment]').click();
  $('[data-increment]').click();
  expect($('[data-count]').text()).toEqual('2');
});
{% endhighlight %}

Now we have to retain the click event listener each time we render the template. We could re-attach the listeners after each render, or we could attach the listener to the `container` and not worry about re-attaching. I'm going to do the latter.

We also need to have a real counter, increment it on each click and display it in the template.
We might do it this way:

{% highlight javascript %}
function CounterWidget(containerId) {
  //I'm omitting code here because it is not changed

  this.initialize = function() {
    var count = 0;
    container.on('click', '[data-increment]', function() {
      count++;
      container.html(renderTemplate(count));
    });
    container.html(renderTemplate(count));
  };
}
{% endhighlight %}

It works!
