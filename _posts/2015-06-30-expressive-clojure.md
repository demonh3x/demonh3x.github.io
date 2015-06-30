---
layout: post
title:  "Expressive Clojure"
date:   2015-06-30 19:30:00
categories: 8thlight
---
It is not the first time that I hear: `Clojure is not a very readable language`.

I must admit, it is very difficult to give proper names in code. But naming is an ability in which the only way to get better is by practicing.

People with some experience in object oriented languages are a bit used to give names to classes, attributes and methods. But in functional languages naming is even more difficult because we are not used to give names to higher-order-functions. The more abstract they are, the more difficult is to name them.

I want to share a piece of Clojure code I wrote today. It is not perfect and I'm sure it can be improved in many ways but I spent some time trying to make it expressive. If you think that Clojure code cannot be very readable, I hope this will change a bit your opinion.

{% highlight clojure %}
(defn play [board]
  (ask-until-is-one-of (empty-spaces board)))

(defn- ask-until-is-one-of [empty-spaces]
  (let [empty-space? #((set empty-spaces) %)
        choices      (repeatedly #(ask-for empty-space?))]
    (print-to-choose-from empty-spaces)
    (get-first empty-space? choices)))

(defn- ask-for [empty-space?]
  (let [choice (ask-a-number)]
    (when-not (empty-space? choice)
              (println "That space is not empty!"))
    choice))
{% endhighlight %}

In order to `play [board]`, I `(ask-until-is-one-of (empty-spaces board))`.

I want you to also notice that when `ask-until-is-one-of [empty-spaces]` the `choices` are `(repeatedly #(ask-for empty-space?))`. And the result is `(get-first empty-space? choices)`.

To `ask-for [empty-space?]`, I get the `choice` by `(ask-a-number)` and `(when-not (empty-space? choice) (println "That space is not empty!"))`.

It still is far from human language, but I think it could be understood by non-programmers.
