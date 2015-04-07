---
layout: post
title:  "Higher-order functions I"
date:   2015-04-08 00:30:00
categories: 8thlight
---
Having the possibility to use functions as values in a programming language enables us to make [higher-order functions][hof]. That's a fancy way of saying that we can do **functions that receive functions as parameters or return functions**. Having that feature unveils interesting benefits.

[hof]: http://en.wikipedia.org/wiki/Higher-order_function

Let's start with a problem I faced today in Ruby. Let's suposse we have a class like this:

{% highlight ruby %}
class Game
  def is_finished?
    # code ommited
  end

  def result
    # code ommited
  end
end
{% endhighlight %}

The `is_finished?` method tells us if the `Game` has concluded. Returning `false` means that there is no result yet and if you call the `result` method it may return `nil` or something without real meaning. You have to wait for `is_finished?` to return `true` to be able to see the real `result`.

That means that every user of that class is going to contain this kind of code:

{% highlight ruby %}
if game.is_finished?
  do_something_with game.result
end
{% endhighlight %}

Each time some part of the code needs to access to `game.result`, it is forced to check if the `game.is_finished?` before it can do something with the result. 

Something smells in there... and not only because the users are forced to duplicate the if statement, but also because it seems to violate [tell don't ask][tda]: the users are asking the game for two things instead of telling it what to do. 

[tda]: http://c2.com/cgi/wiki?TellDontAsk

So, I followed those smells and using higher-order functions (blocks/lambdas in Ruby) came up with this idea:

{% highlight ruby %}
class Game
  def when_finished(&block)
    block.call(result) if is_finished?
  end

  private
  def is_finished?
    # code ommited
  end

  def result
    # code ommited
  end
end
{% endhighlight %}

That changes the way the users interact with it:

{% highlight ruby %}
game.when_finished {|result| do_something_with result}
{% endhighlight %}

With this we have reduced the smells we were noticing. 

But there is more. We forgot the `else`. What if the user of that code wants to do something else when the game is not finished? Because the implementation of `when_finished` evaluates to `nil` when is not finished, we can do the following:

{% highlight ruby %}
game.when_finished {|result| do_something_with result} || do_something_else
{% endhighlight %}

That works as we expect because the `||` operator is [lazy][lazy]. It only evaluates the right operand (`do_something_else`) when the left operand (`game.when_finished {|result| do_something_with result}`) is falsey. 

[lazy]: http://en.wikipedia.org/wiki/Lazy_evaluation

This syntax may be a bit forced, and this might be evolved to something else, *maybe*.
