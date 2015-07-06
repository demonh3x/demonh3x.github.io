---
layout: post
title:  "Game sequence - part I"
date:   2015-07-06 19:15:00
categories: 8thlight
---
While working in the Clojure tic-tac-toe, to decouple the game evolution (how the game changes from one turn to the next) from the representation of it (user interface), I represented the game as a sequence of game-states.

Let's use a trivial example: a countdown.

First, I needed to have a function that takes the previous `count` and returns the next `count`.

{% highlight clojure %}
(defn countdown [count]
  (dec count))
{% endhighlight %}

To have the complete sequence is as easy as iterating over it:

{% highlight clojure %}
(defn game [initial-count]
  (iterate countdown initial-count))
{% endhighlight %}

That will give us an infinite sequence, but we need to stop when the game has been finished: in this case when we arrive at 0. Let's define that condition:

{% highlight clojure %}
(defn finished? [count]
  (<= count 0))
{% endhighlight %}

And we need now the sequence to that point. I could use `take-while` for that purpose:

{% highlight clojure %}
(defn game [initial-count]
  (take-while #(not (finished? %))
              (iterate countdown initial-count)))
{% endhighlight %}

But if we run that, the output is:

{% highlight clojure %}
user=> (game 5)
(5 4 3 2 1)
{% endhighlight %}

What happened to the `0`? We want to display the `initial-count` and the last count where the game has `finished?`!

`take-while` is not suitable for us. I've been looking for a built-in function that would include that `0`, but I haven't found it.

We need to implement it ourselves. We will see that in the following parts.
