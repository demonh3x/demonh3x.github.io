---
layout: post
title:  "Introduction to Gradle"
date:   2015-08-26 20:00:19
categories: 8thlight
---
The most common thing to do when learning to program is to use an [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment). It will help you focus on the programming language which is hard enough to learn.
On the other hand, it will create a dependency on the tool. They only way you will know how to do most of the tasks will be through the IDE. It happened to me.

But when you start working with more developers, you should not expect everyone to use the same IDE as you. That doesn't mean to avoid using any tool to automate difficult or repetitive tasks. But there needs to be a consensus on which tool. The basic things are automating the procedure to run the tests or building the source code into a distributable package.

Ruby has a very easy-to-use tool named [Rake](https://github.com/ruby/rake). But for Java, things are not as simple. A couple of tools are widely used: [Maven](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) and [Gradle](https://docs.gradle.org/current/userguide/tutorial_java_projects.html).

The first thing I tried was to create an initial template with a very simple example. So I could use that experience to increase my knowledge about the tool and to have something easy to setup for future projects. It is something that can be useful for anybody starting with Gradle, so you can find that [template in my github](https://github.com/demonh3x/gradle-helloworld).

For more complex setups that require packaging into a jar with all its dependencies, you can take a look at [one of my repositories as an example](https://github.com/demonh3x/CobSpecServer/blob/master/build.gradle)
