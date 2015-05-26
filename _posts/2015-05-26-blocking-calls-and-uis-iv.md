---
layout: post
title:  "Blocking calls and UIs IV"
date:   2015-05-26 22:00:00
categories: 8thlight
---
In the [previous post][bc-uis-3] we solved the `computer vs computer` usage. Let's face the interleaving of `computer` and `human` in this one.

[bc-uis-3]: /8thlight/2015/05/11/blocking-calls-and-uis-iii.html
 
With only `humans` we had the following API:

{% highlight ruby %}
class Game
  def move(location)
  def board
  def is_finished?
  def winner
end
{% endhighlight %}

And with only `computers` we had the following:

{% highlight ruby %}
class Game
  def move
  def board
  def is_finished?
  def winner
end
{% endhighlight %}

We need to support both ways of usage in a generic way. Let's start by describing what they do and find the things they have in common.

We are calling both `move` methods to advance to the next player. But, for a `human` the game requires a `location` to play. If we keep the `location` parameter, what do we send when the computer is playing? `nil`?. And more importantly, how do we know the computer is playing to send `nil`? Do we need to add the `current_player` method to `Game`? That will make the UIs know and duplicate some of the `Game` logic and violates the [single responsibility principle][srp].

[srp]: http://en.wikipedia.org/wiki/Single_responsibility_principle

We have another option, we can keep the `move` method without any parameters and provide that location in some other way. But in what way? If the `Game` knows when it needs the `location` value, makes sense for the `Game` to request it when is needed. What object should `Game` request for the `location`? The `location` comes from the user through the UI, but the `Game` shouldn't know about any specific UI. We need to use the [dependency inversion principle][dip] here: `Game` depends on an abstraction to get the `location` and the specific UI will provide its implementation for it.

[dip]: http://en.wikipedia.org/wiki/Dependency_inversion_principle

{% highlight ruby %}
class Game
  def initialize(user)
  def move
  def board
  def is_finished?
  def winner
end
{% endhighlight %}

We have to keep in mind that in a `Gui`, if the `Game` asks to `get_location`, the user may not have selected a `location` yet. And we cannot block for the same reasons we talked about in the [previous post][bc-uis-3]. That means that the object that provides the `location` could not be ready to be queried. 

An implementation could be:

{% highlight ruby %}
class GuiUser
  def when_user_selected_location(location)
    @location = location
    @is_ready = true
  end

  def is_ready?
    @is_ready
  end

  def get_location
    @is_ready = false
    @location
  end
end
{% endhighlight %}

In the `Cli` things are much easier. We know that each time we `ask_to_select_location` it will block and return a valid `location`. The implementation could be as simple as:

{% highlight ruby %}
class CliUser
  def is_ready?
    true
  end

  def get_location
    ask_to_select_location
  end
end
{% endhighlight %}

With that, we can have all modes working.
