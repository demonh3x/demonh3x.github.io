---
layout: post
title:  "Optimizing with metaprogramming"
date:   2015-04-23 8:30:00
categories: 8thlight
---
Most times, optimization is a trade-off. You sacrifice something, usually readability and simplicity, in favor of a piece of code that executes faster. Is your judgment who decides if that speed gain is worth the loss in the other aspects. This post is about a technique that focuses only on maximizing speed. It is just another tool in our toolbox. It may be used if necessary given the trade-offs. 

I came up with this idea while facing the optimization phase on the AI for tic tac toe. I'll use [that example][commit].

[commit]: https://github.com/demonh3x/tictactoe.rb/commit/d8f4d16e3ee4d540e06b373d0612ee797b58e332

The code processed the following data; an array containing the `marks` where each player has made a move, and the groups of indexes in `marks` that conform a `line`.

For a 3 by 3 board that data would be:

{% highlight ruby %}
marks = [
  :x,  :x,  :x,
  nil, :o,  :o,
  nil, nil, nil,
]
lines_3_by_3 = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],

  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],

  [0, 4, 8],
  [2, 4, 6],
]
{% endhighlight %}

One of the requirements is also to support a 4 by 4 board, so my generic implementation was:

{% highlight ruby %}
def winner
  @winner ||= lines
    .map{|line| marks_in line}
    .select{|line_marks| are_the_same? line_marks}
    .map{|marks| marks.first}
    .select{|mark| mark != nil}
    .first
end

def marks_in(line)
  line.map{|location| marks[location]}
end

def are_the_same?(line_marks)
  line_marks.all?{|mark| mark == line_marks.first}
end
{% endhighlight %}

Given a simple integration scenario, the profiler measurements pointed that the `winner` method was being called 17280 times and roughly **75%** of the processing time was spent there. 40% of which was spent on `select` calls and 30% on `map` calls. Those `map` and `select` calls are very expensive, we need to get rid of those.

When we make a piece of code more generic, we are extending the amount of scenarios solved by that code. If given good names it also creates an abstraction layer that helps human reasoning. But generalizing comes with a small cost in performance. Most of the times that cost is completely irrelevant. 75% is not irrelevant.

Basically, what we are doing in those lines is looking at the `marks` array by the `line` indexes to get the mark that is occupying a full `line`. 
Lets try to think closer to low-level: remove generalizations and just focus on 3 by 3 board:

{% highlight ruby %}
def winner
  m = marks
  @winner ||= (
    ((m[0] != nil) && (m[0] == m[1]) && (m[1] == m[2]) && m[0]) ||
    ((m[3] != nil) && (m[3] == m[4]) && (m[4] == m[5]) && m[3]) ||
    ((m[6] != nil) && (m[6] == m[7]) && (m[7] == m[8]) && m[6]) ||
    ((m[0] != nil) && (m[0] == m[3]) && (m[3] == m[6]) && m[0]) ||
    ((m[1] != nil) && (m[1] == m[4]) && (m[4] == m[7]) && m[1]) ||
    ((m[2] != nil) && (m[2] == m[5]) && (m[5] == m[8]) && m[2]) ||
    ((m[0] != nil) && (m[0] == m[4]) && (m[4] == m[8]) && m[0]) ||
    ((m[2] != nil) && (m[2] == m[4]) && (m[4] == m[6]) && m[2])
  ) || nil
end
{% endhighlight %}

The `&&` and `||` operations in Ruby may require some explanation:

{% highlight ruby %}
false || true                 #=> true
false || false                #=> false
false || :x                   #=> :x
false || (puts "evaluated")   #=> nil //out: evaluated
nil   || (puts "evaluated")   #=> nil //out: evaluated

true  || true                 #=> true
true  || false                #=> true
true  || :x                   #=> true
true  || (puts "evaluated")   #=> true
:x    || (puts "evaluated")   #=> :x

#Conclusion:
#If the expression on the left is truthy it will be the result.
#Otherwise; the expression on the right will be the result.

false && true                 #=> false
false && false                #=> false
false && :x                   #=> false
false && (puts "evaluated")   #=> false
nil   && (puts "evaluated")   #=> nil

true  && true                 #=> true
true  && false                #=> false
true  && :x                   #=> :x
true  && (puts "evaluated")   #=> nil //out: evaluated
:x    && (puts "evaluated")   #=> nil //out: evaluated

#Conclusion:
#If the expression on the left is falsey it will be the result.
#Otherwise; the expression on the right will be the result.
{% endhighlight %}

Measuring again with the profiler, given exactly the same integration scenario, we see that the `winner` method now spends only **9%** of the total time, which is an immense improvement.

But, by doing that, we lost the generic properties of the initial algorithm; it will only work for the 3 by 3 board.

We need to regain the correct behavior independently of the `lines`. Generalizing the code will slow us down again. Now, metaprogramming comes in handy:

{% highlight ruby %}
def self.each_group_of(count, collection)
  last_index = collection.size - (count - 1) - 1
  (0..last_index).each do |index|
    group = collection.slice index, count
    yield group
  end
end

def self.get_code_for_winner_in(line)
  code = "(marks[#{line.first}] != nil) && "
  each_group_of(2, line) do |a, b|
    code += "(marks[#{a}] == marks[#{b}]) && "
  end
  code += "marks[#{line.first}]"
  code
end

def self.get_winner_code(lines)
  lines
    .map{|line| "(#{get_code_for_winner_in line})"}
    .join(" || ")
end

class_eval(%Q{
  def winner
    @winner ||= (#{get_winner_code lines}) || nil
  end
})
{% endhighlight %}

Now, we are generating the correct code to handle any type of board. And that code will execute much faster!
