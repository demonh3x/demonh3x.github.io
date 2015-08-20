---
layout: post
title:  "Complexity dimensions - FizzBuzz part III"
date:   2015-08-20 18:30:19
categories: 8thlight
---
I want to stand out a similarity at this point with how I implemented each one of the previous `simple dimensions`: I added first a test that forced me to have a *special case* (if statement). Then a second test that started to unveil some duplication but not enough to be sure. And finally, a third test that made the duplication obvious so I could refactor to the generic implementation.

The similarity is that the `Fizz dimension` and the `Buzz dimension` are like the second test on a `simple dimension`: I have two examples of the names to combine. They unveil some duplication but not enough to be sure. I need to introduce a third example to make the duplication obvious and refactor to the generic implementation.

Based on that, I carry on (7:41-11:00) implementing a feature that is not mentioned in the requirements: the `Bang dimension`. The following tests guide it:

{% highlight ruby %}
7 -> 'Bang'
14 -> 'Bang'
28 -> 'Bang'
{% endhighlight %}

And finally, from then to the end (11:00 - 16:21), I implement the generic `simple dimensions` combination helped by the tests:

{% highlight ruby %}
15 -> 'FizzBuzz'
21 -> 'FizzBang'
35 -> 'BuzzBang'
105 -> 'FizzBuzzBang'
{% endhighlight %}

This way I end with this implementation:

{% highlight ruby %}
def fizzbuzz(prompt)
  names = []

  names << 'Fizz' if prompt % 3 == 0
  names << 'Buzz' if prompt % 5 == 0
  names << 'Bang' if prompt % 7 == 0

  return names.join unless names.empty?
  prompt
end
{% endhighlight %}

There is still duplication in there, but it is not so related to demonstrate the `complexity dimensions` concept. Just for curiosity's sake, It could have been removed this way:

{% highlight ruby %}
FACTORS = {
  'Fizz' => 3,
  'Buzz' => 5,
  'Bang' => 7
}

def fizzbuzz(prompt)
  names = []

  FACTORS.each do |name, divisor|
    names << name if divisible_by?(prompt, divisor)
  end

  return names.join unless names.empty?
  prompt
end

def divisible_by?(number, divisor)
  number % divisor == 0
end
{% endhighlight %}

You might argue that the `Bang dimension` was not in the requirements and therefore it shouldn't be there. I agree. It served the purpose of unveiling enough duplication to be sure what would be the generic implementation.

We can remove the `'Bang => 7` factor and its related test if we want. Or, even better would have been to conform to the [Open-Closed principle](http://c2.com/cgi/wiki?OpenClosedPrinciple) by, for example, creating a class and passing the factors in the constructor.
