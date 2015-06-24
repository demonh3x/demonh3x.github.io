---
layout: post
title:  "Simple design"
date:   2015-06-24 18:10:00
categories: 8thlight
---
There are [4 rules][rules] that are guidelines when one is seeking for simple design:

[rules]: http://c2.com/cgi/wiki?XpSimplicityRules

* Tests pass (the code works).
* Expresses intent (is very easy to understand and find the places where the changes should be done).
* No duplication of knowledge.
* Minimal amount of pieces (modules, classes, methods) and code.

Doing TDD, the first rule [is taken for granted][tdd].

[tdd]: http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd

But with the other three rules, things start to be more fuzzy.

I found out some practices that helped me to make my design close to those guidelines:

* Start with [top-down][tdbu] testing to guide me in the direction of the pieces that I will need to solve the problem. That makes me avoid doing [BDUF][bduf].
* Looking for explanations of the problem in human-language and write down the words that are used to describe it. That way I had a vocabulary to describe the concepts in an un-ambiguous and very clear way.
* When adding tests, focusing on their names as if I was describing it to another person and let those to guide the code's API (from [Corey Haines' book][4rulesbook]).

[tdbu]: http://www.softwaretestingstuff.com/2007/10/top-down-testing-vs-bottom-up-testing.html
[bduf]: http://c2.com/cgi/wiki?BigDesignUpFront
[4rulesbook]: https://leanpub.com/4rulesofsimpledesign

I also found out that the definition of simple have different shades for different people. Is interesting to read about their opinions, for example: [Ward Cunninghan][ward] or [Rich Hickey][rich].

[ward]: https://en.wikiquote.org/wiki/Ward_Cunningham#The_Simplest_Thing_that_Could_Possibly_Work
[rich]: http://www.infoq.com/presentations/Simple-Made-Easy

Sometimes I found out other design guidelines pointing me to a bit different directions.

For example: SRP would prefer to have the concerns separated in different places, taking that to the extreme may contradict a bit with:

* `Minimal amount of pieces`: a lot of extremely small pieces.
* `Expressing intent`: very abstract names that are difficult to understand and are not in the domain.

Basically, I found that I'm unable to conform to all the design guidelines I know. I found out that it is a game of trade-offs. I needed to find the sweet-spot having all of them in mind.
