---
layout: post
title:  "Using Clojure code from Java"
date:   2015-08-17 18:46:19
categories: 8thlight
---
Clojure, being a JVM-based language, should be easy to interoperate to and from Java. Using Java from Clojure is [quite well documented](http://clojure.org/java_interop) and there is a lot of available information.
But using Clojure from Java is not as easy as the other way around.

First, because Java does not depend on Clojure, it needs to be loaded. It is easy to load: It is available in [maven central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22clojure%22). Just declare a dependency to it in your build tool or IDE.

You still need to load your own code before being able to use it. To load your Clojure code and its dependencies, there are two ways that I know of:

* Set a directory in the classpath to your Clojure source code. It will be interpreted at Java runtime. I have an [example of this way in a repo](https://github.com/demonh3x/java-clojure-interop)
* Compile your Clojure code and declare a dependency to it in your build tool or IDE. To compile it I used `lein uberjar`. I have another [example of this way in a repo](https://github.com/demonh3x/clj-tictactoe-in-java-server)

Having done all that, you can finally use `Clojure.var` in Java to get the `IFn`/`fn` defined in Clojure. But if it is in other module than `clojure.core`, you need to require it first. And to load a namespace, you need to convert a Java String into the appropriate Clojure Keyword. The following snippet does that:

{% highlight java %}
public static IFn function(String namespace, String functionName) {
  Clojure.var("clojure.core", "require").invoke(Clojure.read(namespace));
  return Clojure.var(namespace, functionName);
}

//Example of usage:
function("my-namespace", "my-function").invoke("argument1", "argument2");
{% endhighlight %}

As an extra, you can also evaluate a String of Clojure code this way:

{% highlight java %}
public static Object evaluate(String clojureCode) {
  return Clojure.var("clojure.core", "eval").invoke(Clojure.read(clojureCode));
}

//Example of usage:
evaluate("(inc 1)");
{% endhighlight %}
