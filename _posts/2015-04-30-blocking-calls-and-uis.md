---
layout: post
title:  "Blocking calls and UIs"
date:   2015-04-30 10:10:00
categories: 8thlight
---
In tic tac toe exists the concept of `players`. The players interact with the game to decide what move to do when it is their turn. 

The initial design I was working on had the following:

{% highlight ruby %}
class Game
  ...

  def finished?
    current_state.is_final?
  end

  def step
    chosen_move = next_player.play(current_state)
    update_state_with(chosen_move)
  end
end

game = Game.new
game.step until game.finished?
{% endhighlight %}

That implies that `next_player.play` should **always return the chosen next state** when called.

For example a player that interacts with the CLI:

{% highlight ruby %}
class CliPlayer
  ...

  def play(state)
    user_input = $stdin.gets #blocks while waiting for the user input
    chosen_move_from(user_input)
  end
end
{% endhighlight %}

That's no problem because a call to `$stdin.gets` blocks until the user has introduced something. That way, we can be sure that the method call to `play` will return always the `chosen_move`.

But when we try to change the `next_player` for one that uses a GUI, where the user interacts in a completely different way, things get complicated very fast. In a GUI, the user generates events on the different components and those events may update other components. Those events are dispatched in an internal loop.

We cannot be sure that a call to `play` on an instance of `GuiPlayer` will always return the `chosen_move` because the user may not have made a move yet. Blocking until having a user input will not work; doing some kind of blocking loop would freeze the GUI and the user would not be able to interact with the GUI to introduce the `chosen_move`.

I'm still trying to find a way that handles both situations well. We'll see how that goes.
