---
layout: post
title:  "Journal - week 22nd May"
date:   2015-05-26 22:00:00
categories: 8thlight
---
Monday 18th
----------
* That night I slept 9 hours. 
* Started doing negamax implementation from pseudo-code, needed to adapt a bit the tests that I had for minimax.
* Standup with apprentices.
* Continued with negamax. Was confused with the negations so I added the tests one by one to discard problems and solve the ones that are appearing.
* I thought I had finished implementing negamax.
* Used negamax for the AI.
* Paired with [Micah][micah] to make the AI in tictactoe value immediate outcomes more. Found some problems caused by some failures on the negamax implementation.
* When arrived home, tried to do the [bubble sort][bs] but I got stuck because I was tired and I was trying a recursive solution (I am not comfortable with it).
* Before going to sleep, while in the [diffused mode][diffused] had some ideas of how to refactor the code of tictactoe. I wrote them down to not forget them.

[micah]: https://twitter.com/slagyr
[bs]: http://en.wikipedia.org/wiki/Bubble_sort
[diffused]: /8thlight/2015/05/13/diffuse-mode.html

Tuesday 19th
------------
* That night I slept 8 hours.
* In the bus, I started refactoring the tictactoe: Introduced immutable cycling iterator (circular linked list) to simplify the knowledge passed to the AI (players information).
* Continued refactoring in the office.
* Standup with apprentices.
* Sam shared his smells on the code.
* More refactoring: Added a bit of player polymorphism and reduced the arguments passed to negamax.
* Noticed an strange lock when the AI played 4x4. Got a bit stuck. Fixed it by incrementing the depth of the options tree. Lost performance.
* Added a rake task for profiling the code.
* Added performance tests to have acceptable criteria and mark the negamax implementation as not good enough yet.
* Sam helped to view the problem on the negamax implementation.
* In the bus home, I wrote two short blog posts about vim plugins (series sugestion from Felipe).
* When arrived home, finished the bubble sort kata with non-recursive solution. Not good enough yet.
* Before going to sleep, while in the [diffused mode][diffused] had some ideas of how to refactor the code of tictactoe and how to improve the kata. I wrote them down to not forget them.

Wednesday 20th
--------------
* That night I slept 7 hours.
* In the bus, I read Clean Code and had the idea of taking pictures of the interesting parts instead of putting a marker between two pages.
* Tried to fix the loss of performance. Did not manage to do it.
* Standup with apprentices.
* Sam paired a bit more with the negamax problem. Realized that lazy enumerators in Ruby are slow! Fixed the problem with the depth, but it is still slow.
* Got back to the previous hand-coded, test-driven implementation of minimax with alpha-beta pruning. Much faster! So much time wasted... :S
* Realized that the algorithm for getting the best moves could be reused.
* Extracted the common code for the Players.
* Paired with Skim on refactoring to see how to face some things that I didn't see how to refactor.
  * Each UI will implement HumanPlayer and connect the creation of it with the core without knowing about specific UIs.
  * Validation of the moves.
  * Sending unused game state to the HumanPlayer (required for player polymorphism)
  * Weird cyclic dependency between part of HumanPlayer and the Game.
* Read Clean Code in the bus home.
* At home, continued with the bubble sort kata. Found a much better path to follow by doing variations analysis (will write a post about it). Still needed improvement.
* Paired with Jarkyn on her kata.

Thursday 21st
-------------
* That night I slept 7 hours. I woke up tired.
* Improved the steps for the bubble sort kata in the bus.
* Standup with apprentices
* Paired with Priya on how to test the code she had to do.
* Started extracting resulting pieces from the refactorings and test-drove them.
* Refactored player polymorphism.
* Started organizing lunch for friday.
* Refactored more small things.
* Updated the gem version to 0.1.1 and published it.
* At home, improved the path for the bubble sort kata and practiced it to not have to think much in the steps. Wrote notes of the steps and what to say while performing it.

Friday 22nd
-----------
* That night I slept 7 hours. I was tired.
* [Micah][micah] had prepared a cohort.
* We started with an introduction to SOLID principles.
* Introduction to Design patterns followed.
* Each one had to choose a design pattern from a list, research about it and then explain it to the rest.
* We had the performance of the katas from each one of us.
* And we finished by a post-writing session. Each one had to choose a topic, research about it and then write a post that another person will read to the rest.
