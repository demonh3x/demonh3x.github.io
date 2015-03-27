---
layout: post
title:  "Indirection"
date:   2015-03-27 16:30:00
categories: 8thlight
---
I was working on the ruby [tic tac toe][ttt] exercise with the [minimax][mm] algorithm. And I started by TDDing the minimax algorithm directly through the payoff score for each given board state. That way I had a class with the algorithm but that class was not conforming to the `Player` contract. So, I had to create a `PerfectPlayer` class that conforms with the `Player` contract and uses the `Minimax` strategy.

[ttt]: http://en.wikipedia.org/wiki/Tic-tac-toe
[mm]: http://en.wikipedia.org/wiki/Minimax

Because there is also a random player, both algorithms can be abstracted to a `Strategy` layer. That means that the `Player` can become an [adapter][adapter] from `Strategy` to `Player`. With that, we have:

[adapter]: http://en.wikipedia.org/wiki/Adapter_pattern

**Game (1 class)** -*uses*-> **Player (1 class)** -*uses*-> **Strategy (N classes)**

This adds a layer of indirection from `Game` to `Strategy` called `Player`. That layer has a cost in complexity, understanding and maintenance. Unless there is a good reason to keep that, it can be simplified to:

**Game (1 class)** -*uses*-> **Player (N classes)**
