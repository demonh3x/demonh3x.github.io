---
layout: post
title:  "Uncertainty"
date:   2015-04-22 10:00:00
categories: 8thlight
---
I was trying to make the AI in the tic tac toe game run fast. It was infinitelly slow. I had the hope that by implementing alpha-beta pruning would make it faster. And it did. After that it took about 15 seconds to make a move in the worse case scenario. It was a big improvement from before. But 15 seconds is too much for a user to wait. Something was not working as expected; it should be much faster.

At that point I was lost. In addition to alpha-beta pruning I tried to use cache to improve the performance, but that did not make any noticeable difference. It was slow! I had no idea about what was wrong with it. I was overwhelmed by the problem and the situation. I really hate that situation.

After disconnecting from the problem for a day or two I was ready to face it again. Now with fresh thoughts. With that and more research on optimised algorithms for tic tac toe I realised that my implementation was only pruning half of what it could. What a stupid mistake! Fixing that improved the speed a lot. And then I decided to use properly the tool for this job: [a profiler][prof].

[prof]: https://github.com/ruby-prof/ruby-prof

The information provided by the profiler pointed to the places where most of the time was spent. Having that precise feedback I was able to try different alternatives and see the difference they made. Now I was making progress! No more uncertainty.

The lesson learned here is: when lost, the things that will help you get through that are: disconnecting from the problem and resting, conversations with fellow programmers, researching for information and using tools to get more feedback on your situation. Seem obvious, but in the middle of a tough situation is harder to realise it.
