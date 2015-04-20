---
layout: post
title:  "Obscure algorithms"
date:   2015-04-20 23:00:00
categories: 8thlight
---
I've been implementing this tic tac toe AI that needs to play in a 4 by 4 board.
It has to be perfect. It has to be fast. And the combination of those things is what makes it difficult. 

One of the problems is that if you use a simple recursive algorithm to analise all the combinations in the game, it will take forever in a 4 by 4 board. They are too many combinations. So, you have to find ways to do less processing. The one I chose was to implement [minimax with alpha-beta pruning][abprun]. But that algorithm is too technical and too academic for someone who has no deep understanding on mathematics like me.

[abprun]: http://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning

Because I needed to implement it in a way that I could be sure it was going to work as expected, I first had to understand what I was trying to do. Understanding why the algorithm works is an interesting challenge.
If you haven't seen [alpha-beta pruning][abprun] before, try to make sense out of what alpha and beta mean and how they work in that context. They are names that depend very heavily on the reader to understand uncommon concepts. I still don't know the real mathematic meaning behind them.

Like most algorithms, the mechanics are very logical and human-like thinking. But the notation and implementation obscures very heavily that meaning.
After spending a full day of work trying to make sense out of it I got an idea. You don't need any mathematics to understand it. You only need to be human and have a bit of logic.

Minimax is based on the thinking that I'm going to choose the best option that I have available and that I can suppose the opponent is going to choose the worse option for me. If the opponent chooses something less bad, that's better for me.

Given those decisions, I know that I am not going to choose the options that are worse for me.
So, if I have the option to choose between a draw or a lose, as soon as I know a path gives the opponent the chance to defeat me I don't need to look further: I prefer the draw for now. Similarly, I don't need to look further into the paths that are not going to improve a previous alternative.

If the opponent can choose between a path that may give me a better outcome than other alternatives, the opponent is going to choose an alternative. As soon as I know the opponent is not going to choose that path with better outcome for me I can stop looking further.

Also, if the opponent has the chance to win, no other option that he may choose is going to be worse than that. As soon as I know he may win, I can stop looking for more options that he may choose in that situation. And the opposite also applies: If I know I can choose to win, I can stop looking for alternatives: I know I'm going to choose to win.

That's it, nothing less and nothing more.
