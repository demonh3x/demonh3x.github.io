---
layout: post
title:  "Game sequence - part II"
date:   2015-07-28 19:30:00
categories: 8thlight
---
A solution that is easy to understand would be using `partition-by`:

{% highlight clojure %}
user=> (partition-by finished? (range 5 -5 -1))
((5 4 3 2 1) (0 -1 -2 -3 -4))
user=> (let [[before after] (partition-by finished? (range 5 -5 -1))]
            (concat before (take 1 after)))
(5 4 3 2 1 0)
{% endhighlight %}

That seems will work. Let's define it.

{% highlight clojure %}
(defn take-while+ [predicate sequence]
  (let [[before after] (partition-by predicate sequence)]
      (concat before (take 1 after))))
{% endhighlight %}

But if we integrate that with our `countdown` function and we run it:

{% highlight clojure %}
(defn game [initial-count]
  (take-while+ #(not (finished? %))
               (iterate countdown initial-count)))

user=> (game 5)
;... blocked ...
{% endhighlight %}

It will get blocked because `(iterate countdown initial-count)` returns an infinite lazy-sequence and partition-by will evaluate every element in the next sequence. Which in this case is equivalent to an infinite loop.
Let's demonstrate it through `take-while+`:

{% highlight clojure %}
user=> (take-while+ (fn [element]
                      (println "evaluated" element)
                      (finished? element))
                    (range 5 -5 -1))
evaluated 5
evaluated 4
evaluated 3
evaluated 2
evaluated 1
evaluated 0
evaluated 0
evaluated -1
evaluated -2
evaluated -3
evaluated -4
(5 4 3 2 1 0)
{% endhighlight %}

You can see how it is evaluating all the elements of the `(range 5 -5 -1)` sequence. I would expect to stop evaluating at `0` (the first element that is `finished?`).
