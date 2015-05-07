---
layout: post
title:  "Blocking calls and UIs II"
date:   2015-05-07 23:00:00
categories: 8thlight
---
In the [previous post][prev] I talked about how I was trying to find a way to handle CLI and GUI for tic tac toe.

[prev]: /8thlight/2015/04/30/blocking-calls-and-uis.html

One mistake that made things more complicated was modeling the concept of `Player`. There is an important distinction between the concept of `Player` and the concept of the `User`. This is another example that **[object oriented programs should model computation][oo-gorilla], [not the real world][oo-not-real-world]**. 

[oo-gorilla]: http://www.defprogramming.com/quotes-by/joe-armstrong/
[oo-not-real-world]: http://programmers.stackexchange.com/questions/137994/does-object-oriented-programming-really-model-the-real-world#comment259465_137994

Although, in the real world, we think of two players interacting with the game, the real computation that we care about at this stage is the `User` of the application. For example; given a game between two `Human Players` doesn't mean that we have two people playing. The computer can't know. It can happen to be only one person playing acting as both players. That means that the computation we care about is that there is a single `User`.

We need to consider too that there is something acting as a `Computer Player`. That means that when it is active we need to let the `User` know what moves it is doing.

There are three examples of usage:

* `human vs human`: the user inputs the move for the first human and sees the change. Then, he/she inputs the move for the second human and sees the change. Finally, the process repeats until the game finishes.
* `human vs computer` (or `computer vs human` the other way around): the user inputs the move for the human and sees the change. Then, he/she sees the change that the computer makes. Finally, the process repeats until the game finishes.
* `computer vs computer`: the user does not input anything, he/she sees the change that the first computer makes. Then, he/she sees the change that the second computer makes. Finally, the process repeats until the game finishes.

All of that is not specific to any UI implementation. That means the code should be developed and tested independently of the UI. Also, the code needs to be valid for all types of UIs; it needs to provide everything any UI needs. It needs to be generic. The only way I know to do that is by thinking in several implementations and seeing if they fit into the design. If they do, it is a valid design. If some implementation not, it can unveil the specific detail that is not valid and maybe point to a way of improving the design.

That has led me to [BDUF][bduf] before, so I followed Kent Beck's advice:
**We will design for today's problems today, and tomorrow's problems tomorrow**. For the time being, we only care about boards of size 3 and 4. We only care about CLI and GUI. And we only care about the players being human or computer.

[bduf]: http://c2.com/cgi/wiki?BigDesignUpFront

We should not face all the complexity at once, so let's worry only on `human vs human` usage for now.

{% highlight ruby %}
class Game
  def move(location)
  def board
  def is_finished?
  def winner
end
{% endhighlight %}

That'll work for CLI, because we can do:

{% highlight ruby %}
class CLI
  def run
    display_board(game.board)
    begin
      game.move(ask_user_to_select_location) 
      display_board(game.board)
    end until game.is_finished?

    display_winner(game.winner)
  end
end
{% endhighlight %}

And will work for GUI too, because we can do:

{% highlight ruby %}
class GUI
  def start
    display_board(game.board)
  end

  def when_user_selected_location(location)
    game.move(location) 
    display_board(game.board)
    display_winner(game.winner) if game.is_finished?
  end
end
{% endhighlight %}

You can see by looking at `CLI` and `GUI` that they have different ways for the user to select the location. In `CLI` we explicitly `ask_user_to_select_location` and in `GUI` we `move` `when_user_selected_location`.

`ask_user_to_select_location` is a synchronous, blocking call. `when_user_selected_location` is an asynchronous, event call.

In the next post from this series we will face the `human vs computer` and `computer vs computer` usages.
