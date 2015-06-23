---
layout: post
title:  "JavaScript ecosystem - part II"
date:   2015-06-23 19:00:00
categories: 8thlight
---
In the [last blog post][ecosystem1] I talked about some tooling to help in the process of developing a JavaScript client application. But I did say nothing about testing. Things get complicated when you get closer to the user interface. In this case, developing an application that will run on the browser is tricky to test.

[ecosystem1]: /8thlight/2015/06/22/javascript-ecosystem-part-1.html

For that, I used [Jasmine][jasmine], [PhantomJs][phantom] and [a Gulp plugin][gulp-jasmine-phantom] to join them. [Jasmine][jasmine] is testing framework with similar syntax to [RSpec][rspec] that has a runner for the browser. [PhantomJs][phantom] is a `headless` browser, which means that it will simulate the browser without running a full browser and interacting manually with it. The [Gulp plugin][gulp-jasmine-phantom] will enable you to automate running those.

[jasmine]: http://jasmine.github.io/
[phantom]: http://phantomjs.org/
[gulp-jasmine-phantom]: https://www.npmjs.com/package/gulp-jasmine-phantom
[rspec]: http://rspec.info/

For example, my `gulpfile.js` contained:

{% highlight javascript %}
var gulp = require('gulp');
var jasmine = require('gulp-jasmine-phantom');

gulp.task('test', function() {
  return gulp
    .src(['vendor/*.js', 'src/*.js', 'spec/*.js'])
    .pipe(jasmine({
      integration: true
    }));
});
{% endhighlight %}

That will load the `vendor`, `src` and `spec` files in that order and then run them in [PhantomJs][phantom]. That way you have access to the `window` and `document` objects as well as using libraries like [JQuery][jquery] to manipulate the [DOM][dom] from your code and tests.

[jquery]: https://jquery.com/
[dom]: https://en.wikipedia.org/wiki/Document_Object_Model

But there is one thing to keep in mind: changes in the [DOM][dom] in one test will persist across during other tests. I guess this is because the [Jasmine][jasmine] runner updates the [DOM][dom] with the test results and [the Gulp plugin][gulp-jasmine-phantom] relies on that. That may break other tests if proper cleanup of the [DOM][dom] is not made `afterEach` test.
