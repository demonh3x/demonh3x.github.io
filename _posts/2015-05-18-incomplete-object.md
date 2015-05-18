---
layout: post
title:  "Incomplete object"
date:   2015-05-18 21:00:00
categories: 8thlight
---
I'm still trying to understand these ideas, how they fit together and what consequences have each one. So this will be more of a presentation of those ideas and the theories I have than a post. The content will be too abstract, I'm sorry about that. I couldn't find a good example to explain them.

While refactoring the GUI code of tic-tac-toe I found an interesting technique that simplify moving things around where they should be and may unveil smells. An example could be found [in my tic-tac-toe][ttt]. 

[ttt]: https://github.com/demonh3x/tictactoe-gui.rb/blob/cd36f2ceb6276aea2d5cd2a4c1a18e48f0a8b18e/lib/tictactoe/gui/qtgui/game_gui.rb

It is based on the idea that objects are bags of functions (methods) that operate with data (attributes). Also, it has in mind that is desirable to have [high-cohesion][hc], which in simplified terms means that each one of the functions use all the data. If you have [high-cohesion][hc] could mean that the data and functions belong together. 

[hc]: http://en.wikipedia.org/wiki/Cohesion_%28computer_science%29

In object-oriented, we have different ways to send that data to the object. We could send those arguments in the methods:

{% highlight ruby %}
class AllArguentsSentInEachMethod
  def method1(a, b, c)
    a + b + c
  end

  def method2(a, b, c)
    (a - b) * c
  end
end
{% endhighlight %}

We could send it as arguments in the constructor:

{% highlight ruby %}
class AllArgumentsSentInTheConstructor
  attr_reader :a, :b, :c

  def initialize(a, b, c)
    @a = a
    @b = b
    @c = c
  end

  def method1()
    a + b + c
  end

  def method2()
    (a - b) * c
  end
end
{% endhighlight %}

Or we could have a combination of both:

{% highlight ruby %}
class SomeArgumentsSentInTheConstructor
  attr_reader :a, :b

  def initialize(a, b)
    @a = a
    @b = b
  end

  def method1(c)
    a + b + c
  end

  def method2(c)
    (a - b) * c
  end
end
{% endhighlight %}

You may prefer one or another depending on how the object collaborates with others.

If you know that the values of `a`, `b` and `c` do not change often, you may prefer to pass all the arguments in the constructor and all the users of the object not worrying about any of those values.

{% highlight ruby %}
def solve_the_problem(object)
  object.method1()
  object.method2()
end

object = AllArgumentsSentInTheConstructor.new(1, 2, 3)
solve_the_problem(object)

#when the values change...
object = AllArgumentsSentInTheConstructor.new(9, 2, 3)
solve_the_problem(object)
{% endhighlight %}

If you know that some values change quite often, you may prefer to pass them as an argument and the clients are going to need to provide them.

{% highlight ruby %}
def solve_the_problem(object)
  c = get_c()
  object.method1(c)
  object.method2(c)
end

object = SomeArgumentsSentInTheConstructor.new(1, 2)
solve_the_problem(object)

#when the values change...
solve_the_problem(object)
{% endhighlight %}

But this seems odd. We are contaminating the `solve_the_problem` procedure with [more responsabilities][srp]: It is obtaining `c`. It is using `c` to complete what `object` needs. And it is using `object` to `solve_the_problem`.

[srp]: http://en.wikipedia.org/wiki/Single_responsibility_principle

We need to provide `a`, `b` and `c` before doing anything with `object`. The problem we have is that those values come from different places. Here [the Builder pattern][builder] may work. But I didn't know it before this problem and it may seem extra complicated, so I did a simpler version of it:

[builder]: http://en.wikipedia.org/wiki/Builder_pattern

{% highlight ruby %}
class IncompleteObject
  attr_accessor :a, :b, :c

  def method1()
    check_for_completion()
    a + b + c
  end

  def method2()
    check_for_completion()
    (a - b) * c
  end

  private
  def is_incomplete?()
    [a, b, c].any?{|attr| attr.nil?}
  end

  def check_for_completion()
    raise "Incomplete object" if is_incomplete?
  end
end
{% endhighlight %}

Having that, there's no much difference from:

{% highlight ruby %}
#in some part of the code:
object = AllArgumentsSentInTheConstructor.new(1, 2, 3)

#in another part of the code:
object.method1()
object.method2()
{% endhighlight %}

To:

{% highlight ruby %}
#in some part of the code:
object = IncompleteObject.new()
object.a = 1
object.b = 2
object.c = 3

#in another part of the code:
object.method1()
object.method2()
{% endhighlight %}

And from:

{% highlight ruby %}
#in some part of the code:
object = SomeArgumentsSentInTheConstructor.new(1, 2)

#in another part of the code:
c = get_c()
object.method1(c)
object.method2(c)
{% endhighlight %}

To: 

{% highlight ruby %}
#in some part of the code:
object = IncompleteObject.new()
object.a = 1
object.b = 2

#in another part of the code:
object.c = get_c()
object.method1()
object.method2()
{% endhighlight %}

With that, I had much more flexibility of moving `object.a = ?`, `object.b = ?` or `object.c = ?` around until I found the place that made more sense, and then changed those to constructor and method arguments again.
