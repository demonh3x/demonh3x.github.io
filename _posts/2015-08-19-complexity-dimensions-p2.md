---
layout: post
title:  "Complexity dimensions - FizzBuzz part II"
date:   2015-08-19 18:30:19
categories: 8thlight
---
Referring to the [kata on the previous post](/8thlight/2015/08/18/complexity-dimensions-p1.html), you can see how in the first minute (0:00 - 1:18), I add three tests that force me to have a correct implementation on one of the `dimensions`: `It returns the same number that has been prompted`.

Basically, the tests are (`Input -> Output`):

{% highlight ruby %}
1 -> 1
2 -> 2
4 -> 4
{% endhighlight %}

You can see how the `3 -> 3` test is missing. That's because 3 would not result in `3`, it would be `Fizz`. That's another `complexity dimension` because it follows a different independent requirement that make the algorithm more complex.

During the next two minutes (1:18 - 3:10), I add the following three tests that unveil enough duplication to implement the generic algorithm for that `dimension`: `It returns Fizz when the number is divisible by 3`.

{% highlight ruby %}
3 -> 'Fizz'
6 -> 'Fizz'
9 -> 'Fizz'
{% endhighlight %}

Two more minutes (3:10 - 5:14) and I have another `dimension`: `It returns Buzz when the number is divisible by 5`. You can see how I hesitate when adding the third test. I was thinking that 15 will result in `FizzBuzz`. But that's another `complexity dimension` because it is the combination of other `complexity dimensions` and thus, makes the algorithm more complex. That's why I chose to go for the 20, which is only `Buzz`.

{% highlight ruby %}
5 -> 'Buzz'
10 -> 'Buzz'
20 -> 'Buzz'
{% endhighlight %}

I then continue combining the `Fizz dimension` and the `Buzz dimension` in a bit more than two minutes (5:14 - 7:41). The tests introduced to force a correct implementation are:

{% highlight ruby %}
15 -> 'FizzBuzz'
30 -> 'FizzBuzz'
45 -> 'FizzBuzz'
{% endhighlight %}

This is the point where the implementation meets the [initial requirements of the kata](http://c2.com/cgi/wiki?FizzBuzzTest):

{% highlight ruby %}
def fizzbuzz(prompt)
  return 'FizzBuzz' if prompt % 5 == 0 && prompt % 3 == 0
  return 'Buzz' if prompt % 5 == 0
  return 'Fizz' if prompt % 3 == 0
  prompt
end
{% endhighlight %}

Although the duplication is not completely obvious, you can see how `prompt % 5 == 0` and `Buzz` appear twice. Same thing with `prompt % 3 == 0` and `Fizz`. Just for the purpose to demonstrate another aspect of the `complexity dimensions` concept I'm not going to stop here.
