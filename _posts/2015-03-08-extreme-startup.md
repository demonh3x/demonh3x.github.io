---
layout: post
title:  "Extreme Startup"
date:   2015-03-08 22:00:00
categories: 8thlight
---
On Friday afternoon we did a session of [Extreme Startup][xs]. It is a competitive programming game.

[xs]: https://github.com/rchatley/extreme_startup

Intro
-----

Each participant has to implement a web API that responds to not-yet-known questions.
There is an API client acting as a referee that asks those questions every few seconds and keeps track of the points for each participant.

Each participant:

- Gets points if the API responds correctly to a question.
- Loses some points if the API responds incorrectly to a question.
- Loses a lot of points if the API is not working when asked a question.

The referee machine also is a web server where each participant can see the if the API has responded correctly, incorrectly or was not working when asked a question. It also has the game scoreboard.

Our approach
------------

I participated with Chris Jordan.
The first thing we thought was: We want to test drive this. How are we going to test our code without the web server nor the referee?

To do that we created a function that received the question and returned the answer.
We tested that function with questions we were loging as the referee was doing them.
That function knew nothing about web stuff.
On the other hand, the web server code received the web request, extracted the question, passed it to the function and formatted the response with the answer.

That way we had a feedback loop much faster than the real environment. We didn't have to wait `random number` seconds for the referee to ask the question we wanted to implement. We were **simulating** the real environment. 

Consequences
------------

Everything was wonderful. We rapidly got way ahead of everyone else because of that rapid feedback.

But our server broke when asked questions it did not expect, so we added some exception handling code to the untested part and thought that would be OK. It was OK for some time, until we started losing some of the advantage.

At the end, [Daniel Irvine][di] and [Jarkyn Soltobaeva][js], who were at 2nd position, surpassed us.
We didn't know why. Our tests passed! 

[di]: http://www.dirv.me/
[js]: http://indykid.github.io/

The worst part is not knowing. We still don't know exactly why. Our tests were very [narrow in scope][testscope] because we were **simulating** the real environment.

We were not looking to the **real world**. 

[testscope]: https://www.facebook.com/notes/kent-beck/making-making-manifesto/857477870951745

