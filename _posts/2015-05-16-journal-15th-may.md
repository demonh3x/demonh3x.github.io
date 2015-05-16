---
layout: post
title:  "Journal - week 15th May"
date:   2015-05-16 21:30:00
categories: 8thlight
---
Monday 11th
-----------
* That night I slept 7 hours. 
* Read XP Explained in the bus.
* Planned what steps to take to create a gem for tic-tac-toe.
* Look for information about how to make gems.
* Created a .gemspec for tic-tac-toe.
* Reorganized modules and files to separate tic-tac-toe in three repositories: Core, CLI, GUI.
* Had some problems after renaming github repository for core.
* Created github repositories for CLI and GUI. 
* Configured Travis for CLI and GUI.
* Added codeclimate for CLI and GUI.
* Lost time because I installed the core gem and then I was refactoring the core gem. And any failures were not being detected because the tests were loading the gem's classes.
* Talked with Skim and other colleagues about git branching strategies.
* Wondered about creating gems for CLI/GUI. Conclusion: They are not libraries to consume, they are binaries to execute.
* Read XP Explained in the bus.
* Published [Blocking calls and UIs III][bcau3] that I wrote over the weekend.

[bcau3]: /8thlight/2015/05/11/blocking-calls-and-uis-iii.html

Tuesday 12th
------------
* That night I slept 9-10 hours.
* Read XP Explained in the bus.
* Planned what steps to take to have similar user experience in GUI and CLI.
* Yak shaved: created git shortcuts for most used commands.
* Added tests for the option to play again: got a bit stuck on asserting that the windows were showing and hiding because the windows were always hidden in tests.
* Tried branching out each small task. Conclusion: too small tasks are a headache, story-size branches seem good.
* When had something that might be enough, asked for the client's feedback.
* Talked a bit with Sam about Ruby: protected, public, private, modules vs inheritance, class instance variables vs instance variables.
* Tried to add bundler cache in Travis to reduce the time of GUI integration, it did not work because the gem was installed manually and bundler did not find it.
* Started refactoring GUI.
* Read XP Explained in the bus.
* At home, wrote [Sleep][sleep] post and published it.
* Felipe proposed to pair for a bit, we started changing inheritance for composition and adding a generic layer for any GUI.

[sleep]: /8thlight/2015/05/12/sleep.html

Wednesday 13th
--------------
* That night I slept 7 hours.
* Finished reading XP Explained in the bus.
* Did standup with apprentices.
* Moved specific GUI widget details to a factory.
* Separated GUI widgets in different files.
* Because the GUI tests depended on Qt framework, I had to change the entry point to access Qt classes a lot of times.
* Generalized GUI widget APIs.
* Talked with Skim about the current refactoring state and the simplest way to detach the UI framework. 
* Had idea for a blog post: setter methods for configuration instead of constructor arguments may help the refactoring stage.
* At home, wrote [Diffuse mode][diffuse] post and published it.

[diffuse]: /8thlight/2015/05/13/diffuse-mode.html

Thursday 14th
-------------
* That night I slept 8 hours.
* Started reading Clean Code in the bus.
* Planned what steps to take to finish the refactor.
* Standup with apprentices.
* Reinforced my feeling on setter methods over constructor arguments in the refactoring stage because simplified moving things around.
* Found a strange thing in the refactoring that felt like a smell: class with no public methods. It was only registering events in the constructor to call the logic and update the display.
* That made me wonder about refactoring towards something like `events (input)` -> `logic` -> `display (output)`
* Had a conversation with Skim about that idea and what approach to focus on. Difficult to separate events from representation because in Qt they are tied together inside the widgets. And decided to have `common Window objects` -use-> `specific GUI objects` -use-> `Qt widgets`.
* A lot of refactoring.
* Useful things in the refactoring:
  * setter methods instead of constructor arguments.
  * integration tests (although had to change after most of the refactorings)
  * changing modification of instance variables for arguments and return values.
* Read Clean Code in the bus.

Friday 15th
-----------
* That night I slept 8 hours.
* Read Clean Code in the bus.
* Standup with apprentices.
* Gave advice to Priya about nervousness in giving talks.
* Had IPM with Skim:
  * Delivered stories: good.
  * Code review: need to do a lot of small improvements, inconsistencies and simplifications.
* Did the estimations again without having to give an immediate answer. Again, it felt comfortable and quite precise.
* We assisted to a talk from Priya on Unix Processes and then another about the impostor syndrome from Daniel and Enrique.
* Had a meeting about improving and concerns.
* Had a small conversation with Uku about doing a free self-organized open-space similar to socrates conference.
* Read Clean Code in the bus.
