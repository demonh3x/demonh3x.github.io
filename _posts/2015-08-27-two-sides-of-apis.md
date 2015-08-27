---
layout: post
title:  "The two sides of APIs"
date:   2015-08-27 20:55:19
categories: 8thlight
---
I remember a specific conversation with [Felipe](http://dev.fesere.de/) about API classification. He described a grid with two properties: prettiness on usage and prettiness of implementation.

You can place any API in that quadrant:

* Ugly APIs in implementation and in usage.
* APIs that have been developed without much care, but provide a nice way of interacting with them.
* APIs with good care on the implementation but forgot about the users of it.
* Nice all-around APIs.

He gave me that feedback while talking about my implementation of a [router for the http server](https://github.com/demonh3x/server.java/tree/master/src/main/java/com/github/demonh3x/server/http/router)
I had a simple and flexible enough API for the requirements, but I did not think much on the users of that code.

For example, the way you had to define the routes was:

{% highlight java %}
new Route(
    new UriMatcher("/method_options"),
    respondAvailableOptions
);

new Route(
    new AndMatcher(
      new MethodMatcher("GET"),
      new UriMatcher("/logs")
    ),
    haveProtectedAccessToLogs
);

new Route(
    new MethodMatcher("GET"),
    readRequestedFile
);
{% endhighlight %}

It is not very handy. By providing a [DSL layer on top](https://github.com/demonh3x/server.java/blob/master/src/main/java/com/github/demonh3x/server/http/router/dsl/Routes.java), now the users could use it in this much-improved way:

{% highlight java %}
onAnyMethodTo("/method_options").will(respondAvailableOptions)

on(GET, "/logs").will(haveProtectedAccessToLogs)

on(GET).will(readRequestedFile)
{% endhighlight %}
