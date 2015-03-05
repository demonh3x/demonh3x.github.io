---
layout: post
title:  "Optimizing the communication"
date:   2015-03-05 15:00
categories: 8thlight vim ruby kata
---
This entry is about this week's task.

The task consisted in being able to do the `Coin Changer` kata driven by tests in Ruby and using Vim. All of that under 7 minutes.

I heard once a person say: "Those craftsmen/craftswomen doing katas super-fast only want to show off. That practice is completely pointless."

In order to be able to do this task, you need to communicate with the computer efficiently.

I introduce next the `Abstract concept` and `An analogy` about comunicating. You can skip those parts and go to `Analysing the task’s aspects` section if you want.

Abstract concept
----------------
When a sender wants to communicate some information to a receiver, some pre-requisites need to exist for that communication to be succesful: 

- The sender needs to know clearly the information he/she wants to communicate.
- They both need to be able to understand and express ideas with a common language.
- As important as the details of the language are, the speed at which both parts can communicate is also important.

An analogy
----------
Lets put the Spanish spoken language as an analogy.

If the speaker doesn't know what he/she wants to communicate, there is no point in trying to speak in any language. The more concise the information is, the better. When the speaker uses too many words to say something, he/she may create confusion to the listener.

The details of the language may vary depending on the zone it is spoken.
It is not exactly the same Spanish spoken in Madrid than the one spoken in Mexico city.
If there is a word that means one thing in Madrid another thing in Mexico city, and you want a perfect communication, the speaker and the listener need to know which meaning they are refering to when using that word. Otherwise there is going to be confusion. 
If they notice something was confused, solving that confusion (if they can) really slows down the communication.

The speed at wich a person can understand spoken Spanish might be a problem.
If the speaker is forced to speak too slow or the listener needs the speaker to repeat the message too many times, the speaker can become frustrated and even stop trying to communicate.

The speed at wich a person can speak Spanish is also a limiting factor. 
It is quite difficult if the speaker's head thinks faster than his/her mouth can articulate words.
Even worse: it is really frustrating when the speaker wants to say something but can't find the words and combine them to say that thing precisely. The best he/she can do in that case is to say something somewhat similar hoping that the receiver understands the idea.

The precision at which a person can speak Spanish is another important thing to consider.
The person might be, for example, stuttering when speaking Spanish. That makes him/her to be constantly correcting himself/herself and take more time than usual to say something. It could also confuse the listener.

Analysing the task's aspects
----------------------------
I introduced the analogy because it has a lot of similarities with the process I faced doing this task. I will explain them as they appear.

To achieve this task, there are many aspects to develop. Lets identify them:

1. Understanding, solving and simplifying the problem 
2. Ruby syntax
3. TDD
4. Vim usage

Those aspects are ordered by their priority in the objective of achieving the task.
It does not make sense to learn Ruby syntax if you don't know how to face the problem yet.
It does not make sense to learn RSpec (to do TDD) if you don't know the Ruby syntax yet.
It does not make sense to use another text editor to go faster if you are struggling with the problem, with the Ruby syntax or with the TDD procedure. 

This doesn't mean you have to master each aspect before getting into the next one. It only means that improving a previous aspect has more time profit than improving on following ones.

Understanding, solving and simplifying the problem
==================================================
Lets start by understand what is the problem we are trying to solve. 

The problem is about calculating the optimal combination of [UK coins][coins] for a money amount. Optimal means that the bigger coins are preferred. The notes are not considered. For example: given 5.98£, the coins should be:

[coins]: http://en.wikipedia.org/wiki/Coins_of_the_pound_sterling#Coins_in_circulation

- Two 2£ coins
- A 1£ coin
- A 0.50£ coin
- Two 0.20£ coins
- A 0.05£ coin
- A 0.02£ coin
- A 0.01£ coin

In [one of my firsts solutions][first-impl] I introduced some [accidental complication][accidental-complication].

I thought the problem needed the notes to be considered. I did not understand the problem. By not considering them I simplified the problem and saved some time.

I made a class with an instance method. That method did not depend on any state of the instance. The result only depends on the parameter. By defining that function without a class I simplified the problem and saved some time.

I used floating point numbers for the coins. That has problems with precision which I solved using BigDecimal. By using integers instead of floating point numbers I simplified the problem and saved some time.

[first-impl]: https://gist.github.com/demonh3x/2f41e9ce5ae0897e0e00
[accidental-complication]: https://vimeo.com/79106557

*Going back to the analogy: this is knowing what I want to say and being concise doing it.*

Ruby syntax
===========
The Ruby syntax is similar to other programming languages but it has it peculiarities. I needed to know which differences has and what they mean before continuing.  

*Going back to the analogy: this is specifying that we are going to use the Madrid's Spanish instead of Mexico's.*

[irb][irb] is a great tool to learn the ruby syntax and to experiment with it. 
The more familiarised you are with the syntax, the faster you can express what you want. By searching in the documentation, experimenting and practicing I became more familiar with the syntax and faster writing it.

[irb]: http://en.wikipedia.org/wiki/Interactive_Ruby_Shell

*Going back to the analogy: this is finding the words and combining them properly to say that thing precisely.*

The coding style is another thing to keep in mind if you don't want to create confusion.

TDD
===
TDD here is a communication enhancer. It makes the communication from the computer to you go much faster. Instead of running the code, trying the different combinations and checking manually, you have all the features tested automatically. You just have to run the tests.

That makes the computer tell you if you are doing something wrong, what are you doing wrong and when you are doing it wrong. 
You can go back a bit and try to do it differently. 
That is quite grumpy, isn't it? It would be interesting if the computer also suggested valid alternatives, but that is out of my knowledge and seems [magical][magic].

[magic]: https://twitter.com/ianbach/status/564938305251192832

The speed of the communication between the programmer and the computer is the time difference between the moment you write code and the computer tells you everytihng is ok.
The more frequent you run the tests, the faster the communication is.
By automating the execution of the tests after saving, I made the communication faster.

If I would need to improve this, I would follow [Kent's suggestion][save-bottleneck] by running the tests each time a key is pressed.

[save-bottleneck]: https://twitter.com/KentBeck/status/570682400948260864

*Going back to the analogy: this is making the listener to understand and respond faster.*

The [transformation priority premise][tpp] suggests you do the steps that require less effort firts. By doing that, the code was evolving smoother and faster.
 
[tpp]: http://blog.8thlight.com/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html
 
Vim usage
=========
At some point, if you are really confortable with the previous aspects, the text editor may get in the way of transmitting the information to the computer.

Learning to use vim feels counterintuitive. Just to get as productive in vim as in your current text editor, you have to learn a lot of shortcuts and be confortable with the modes. That takes time and practice.

Why is writing in vim faster than in a graphical text editor? That's because all the shortcuts are as easy to press as any letter. That makes your hands not to lose time moving to the mouse or arrows and then lose more time going back to the writing position.

*Going back to the analogy: this is making your mouth articulate words faster to keep up with the thoughts in your head.*

You have to be careful to learn properly the  actions. Because if you learn that moving a finger to press a key to do an action, but that key doesn't do that action, you are going to be correcting yourself very often and losing time because of it. By polishing my finger moves I saved some time.

*Going back to the analogy: this is stuttering while trying to say something.*

The Result
----------
<iframe src="https://player.vimeo.com/video/121234763" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/121234763">Coin Changer Kata</a> from <a href="https://vimeo.com/user33522377">Mateu Adsuara</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

That's 5 minutes 15 seconds. And there is yet room for improvement.

Remember: 
`Strive for progress, not perfection`
