---
layout: post
title:  "Blocking calls and UIs III"
date:   2015-05-11 21:00:00
categories: 8thlight
---
In the [previous post][bc-uis-2] we solved the `human vs human` usage. Let's face the `computer vs computer` usage now. I'm choosing `computer vs computer` after `human vs human` because it is simpler than introducing `computer vs human` directly. If we did that, we would have to think how to do the computer and also how we interleave computer and human. By doing `computer vs computer` first, we are deferring the interleaving complexity for now.

[bc-uis-2]: /8thlight/2015/05/07/blocking-calls-and-uis-ii.html

* `computer vs computer`: the user does not input anything, he/she sees the change that the first computer makes. Then, he/she sees the change that the second computer makes. Finally, the process repeats until the game finishes.

Because the user does not input anything, the argument `location` on the `Game.move` method is not relevant, so let's forget about it for now. If we did that, we would have the following `Game` class:

{% highlight ruby %}
class Game
  def move
  def board
  def is_finished?
  def winner
end
{% endhighlight %}

The `CLI` is simple; we just loop and print.

{% highlight ruby %}
class CLI
  def run
    display_board(game.board)
    do
      game.move
      display_board(game.board)
    until game.is_finished?

    display_winner(game.winner)
  end
end
{% endhighlight %}

But when we would implement the GUI we would have a problem; we were relying on the `when_user_selected_location` event to `display_board` and `display_winner` after each move. Now, we cannot depend upon that event. We want something automatic. We might think in using a loop like the one in the `CLI`, but that would not work.

To understand why, we need to know that in a `GUI` there are a lot of components. Events on one component have consequences on others. Most `GUI` work with an [event loop][event-loop] that, very roughly, iterates over all the components to update them, notifying any events that need to be processed and repainting if their aspect has changed. If we block the [event loop][event-loop] by doing a long computation or running another very long loop, the `GUI` will stop responding because the components are no longer notified with the events and are neither repainted.

[event-loop]: http://en.wikipedia.org/wiki/Event_loop

We would like to execute something inside that [event loop][event-loop] without stopping its iteration. It depends on the GUI framework but, most of them provide an event that enables the execution of code very frequently. Let's refer to it as `update`. We could use it like this:

{% highlight ruby %}
class GUI
  def start
    display_board(game.board)
  end

  def update
    game.move
    display_board(game.board)
    display_winner(game.winner) if game.is_finished?
  end
end
{% endhighlight %}

That's good enough for our case because the `Game.move` method does not take more than 1 second to finish in the worst case scenario. That means that the `GUI` may not be responding for 1 second, but we are sure it will resume responding after that.

If the `Game.move` method would take more than 1 second and the blocking on the `GUI` should not happen at all, we would need to take a look at more complicated stuff: threads.

In the next post we will face the interleaving of `human` and `computer`.
