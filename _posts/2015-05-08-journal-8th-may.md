---
layout: post
title:  "Journal - week 8th May"
date:   2015-05-08 21:30:00
categories: 8thlight
---
This is the first of the weekly series on my newly started journal containing the small tasks I do as the `current actions` *horizon* (explained in the [perspective blog post][perspective]). This came out of a suggestion from my co-mentor [Felipe][fsere] about providing concrete examples to the generic idea of *horizons*.

[perspective]: /8thlight/2015/05/06/perspective.html
[fsere]: http://dev.fesere.de/

Tuesday 5th
-----------
* That night I slept 10-9 hours. 
* Read [XP explained][xp] in the bus to the office.
* Planned small tasks to follow: it felt productive.
* Showed the result in the tictactoe GUI.
* Avoided moving in tictactoe to places already occupied.
* Got a bit stuck refactoring duplication of copy-pasting the buttons in the GUI  (generalizing click events). Achieved it by receiving the sender object of the event in the slot (Qt) as suggested by [Christoph][christoph] (shout out!)
* Added complexity for compatibility with computer player in tictactoe
* Lost (or invested) 30 minutes organizing the following tasks.
* Introduced GUI tests.
* Separated old previous CLI code that is reusable and adapted tests for it.
* Used new generic for UIs tictactoe code for CLI code.
* Removed old code.
* Read [XP explained][xp] in the bus back home.
* In the afternoon I wrote 2 blog posts: [Getting things done][gtd] (reused part of my own transcription of the talk from David Alen) and [Perspective][perspective]

[xp]: http://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0201616416
[gtd]: /8thlight/2015/05/05/getting-things-done.html
[christoph]: https://twitter.com/ChristophGockel

Wednesday 6th
-------------
* That night I slept 8 hours.
* Read [XP explained][xp] in the bus to the office.
* Planned what to do (steps to get computer player working)
* Spiked timer for GUI required to make the updates of the computer player without locking the GUI.
* Erased spike code and started test-driving it
* Spent 30 min approximately with [Priya][priya] helping with naming, intent and a bit of refactoring.
* Asked Nathan and [Makis][makis] for advice on testing order of calls. Conclusion: don't, better to have end-to-end that make sure.
* Added GUI top-botton, end-to-end tests that helped know what to do next and when I'm done.
* Starting adding tictactoe options in the same GameWindow but then refactored to different window because it more decoupled and easier to test.
* Had warnings for having multiple Qt layouts on top of the widget hierarchy.
* Had intermitent, non-always reproducible segmentation faults from Qt bindings for Ruby when trying to resize GameWindow. Spent some time trying to reproduce and isolate cause. I had no luck with it. Separating options into another window avoided those to appear.
* Created pull request of the GUI to review.
* Travis CI was not building because Qt bindings was not specified in the Gemfile.
* Removed obsolete code.
* Fixed properties and regression tests that depended on obsolete code.
* Read [XP explained][xp] in the bus back home.

[priya]: https://priyapatil101.wordpress.com/
[makis]: http://maikon.github.io/

Thursday 7th
------------
* That night I slept 10-9 hours.
* Read [XP explained][xp] in the bus to the office.
* Did standup with apprentices.
* Made Travis CI run code by adding Qt bindings in the Gemfile but that made the integration slow (15 minutes each execution) and the test were failing because it couldn't connect to X server.
* Ignored GUI tests and updated pull request to send it via mail for review.
* Had doubts about if the testing GUI was the correct approach. [Skim][skim] suggested is good and I should run all the tests In CI.
* [Sam][sam] suggested using [xvfb][xvfb] for running GUI tests in Travis CI. When I tried, it worked perfectly (shout out!) 
* Because I was not willing to wait 15 minutes each time I would like to try a change in CI, I started creating my own CI machine locally with Vagrant but that felt [yak shaving][yak-shaving].
* Updated higher-order functions blog post with feedback received.
* [Felipe][fsere] suggested do this series of posts after reading [perspective blog post][perspective] (shout out!)
* Wrote a bit of [blocking calls and UIs II][bc-and-uis-2].
* Read [XP explained][xp] in the bus back home.
* Spent 2 more hours finishing [blocking calls and UIs II][bc-and-uis-2] at home.

[skim]: http://skim.la/
[sam]: http://hanster.github.io/
[xvfb]: http://docs.travis-ci.com/user/gui-and-headless-browsers/
[yak-shaving]: http://en.wiktionary.org/wiki/yak_shaving
[bc-and-uis-2]: /8thlight/2015/05/07/blocking-calls-and-uis-ii.html

Friday 8th
----------
* That night I slept 7 hours.
* Read [XP explained][xp] in the bus to the office.
* Started preparing the IPM: thinking how to drive it, having a browser with 8th Light's tool for IPMs, links to tictactoe code, blog posts, etc...
* Got distracted in [Slack][slack] by talking about the food ordering process for 8LU and came up with the idea of doing a tool to simplify it.
* Had my IPM with [Skim][skim]. The *client* side of him was happy with what was delivered and suggested some changes to refine what he preferred. The *mentor* side reviewed the code and suggested some refactorings.
* We tried the estimation process without the pressure of having to give an answer at the same moment. That felt more comfortable.
* Asked for feedback on the idea of the tool for the food ordering process and another tool for doing the pomodoro technique with a team.
* [Enrique][ecomba] facilitated a debate about trust and some shared experiences.
* We had a conversation with [Makis][makis], [Christoph][christoph] and [Skim][skim] about Elixir, a language build on top of Erlang.
* Had the monthly retrospection with [Skim][skim] and [Felipe][fsere] and we concluded with the following goals (in [perspective][perspective]: `current projects` *horizon*):
  * Minimize risk by splitting stories and scope.
  * Find heuristics to guide my estimates.
  * Improve my git usage by branching more.

[slack]: https://slack.com/
[ecomba]: http://ecomba.pro/
