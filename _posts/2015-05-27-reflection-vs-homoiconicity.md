---
layout: post
title:  "Reflection VS Homoiconicity"
date:   2015-05-27 22:00:00
categories: 8thlight
---
When we make the code more generic, we are making it more flexible and reusable in the sense that the code can handle more usages.

The first tool in a programming language we have for that is the ability to express abstractions. But when we push those abstractions to the limits, we might find the need to modify the code itself in order to handle even more usages.

Both `Reflection` and `Homoiconicity` provide ways to access the language constructs and enable the developer to reshape or adjust the code within itself.

`Reflection` is the way that some languages, like Java or Ruby, provide access to the code itself. They provide an API with methods to find and do almost anything the designers of the language thought the developer might want to do. If the language designers didnt't think of something, doing it will not be possible because it will not be included in the API.

On the other hand, `Homoiconicity` means that the code can be represented in terms of the basic data types. For example in Lisp, from which Clojure derives, the code to do a function call is a list. That list has the function to call as the first element. The remaining elements in that list are the arguments for the function. Simplifying the syntax in that way makes modifying the code as simple as modifying a list. 
