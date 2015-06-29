---
layout: post
title:  "Dilemma on testing functional code"
date:   2015-06-29 19:00:00
categories: 8thlight
---
Currently I'm thinking about a dilemma in different ways to test Clojure code.

If we assume that functional code is based on composing another functions, how do I test the composition without testing the composed pieces again?

In object oriented we avoid that by injecting the collaborators and replacing them for test doubles. And there is a reason for that: [Joe B. Rainsberger explains][scam-post] [it better than me][scam-video].

[scam-post]: http://blog.thecodewhisperer.com/2010/10/16/integrated-tests-are-a-scam/
[scam-video]: https://vimeo.com/80533536

But shall I avoid that in functional? Let's put an example. Let's say I have two functions that check against a tic-tac-toe board: `full?` and `has-winner?`. Their tests are:

{% highlight clojure %}
(describe "full?"
          (it "is false when there is one empty space"
              (should= false
                       (full? [:o :x :o
                               :x :x :o
                               :x :o :empty])))

          (it "is false when there are two empty spaces"
              (should= false
                       (full? [:o     :x :o
                               :x     :x :empty
                               :empty :o :x])))

          (it "is true when there is no empty space"
              (should= true
                       (full? [:o :x :o
                               :x :x :o
                               :x :o :x]))))

(describe "has-winner?"
          (it "is false in a board with no marks"
              (should= false
                       (has-winner? [:empty :empty :empty
                                     :empty :empty :empty
                                     :empty :empty :empty])))

          (it "is true in a board with the first horizontal line occupied by x"
              (should= true
                       (has-winner? [:x     :x     :x
                                     :empty :empty :empty
                                     :empty :empty :empty])))

          (it "is o in a board with the third vertical line occupied by o"
              (should= true
                       (has-winner? [:empty :empty :o
                                     :empty :empty :o
                                     :empty :empty :o])))

          ;More tests ommited: each line that makes a player win and its edge cases
          ;...
{% endhighlight %}

And now let's implement the `finished?` function. The implementation of which should be similar to:

{% highlight clojure %}
(defn finished? [board]
  (or (full? board) (has-winner? board)))
{% endhighlight %}

How would you test that?

* Would you find a way to stub the collaborators (`full?` and `has-winner?`)? Wouldn't that be over-complicated for such a simple function?
* Would you put much less examples? How do you make sure the correct implementation is in place?
* Would you repeat all the same input examples that have been used in `full?` and `has-winner?` again? Isn't that duplication?
* Would you move all the tests examples from `full?` and `has-winner?` to `finished?` and remove the tests for `full?` and `has-winner?` (Because they are utility functions). Wouldn't that be a lot of tests with different behaviors for only one function?
