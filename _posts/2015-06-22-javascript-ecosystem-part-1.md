---
layout: post
title:  "JavaScript ecosystem - part I"
date:   2015-06-22 20:00:00
categories: 8thlight
---
I'm starting a JavaScript project and I had to setup a lot of tools for a proper developing environment.

First, [npm][npm] is the package manager that manages your project's dependencies. It is like [bundler][bundler] in Ruby.

[npm]: https://www.npmjs.com/
[bundler]: http://bundler.io/

In its configuration file you can define what your project needs:

{% highlight json %}
{
  ...
  "dependencies": {
    ...
  },
  "devDependencies": {
    ...
  }
  ...
}
{% endhighlight %}

After that, if you don't want to have the developing code all in one file, you need some kind of module system that makes the code that is depended upon available.

The simplest thing could be:

{% highlight html %}
<html>
  <head>
    ...
    <script src="vendor/vendor-1.js"></script>
    <script src="vendor/vendor-n.js"></script>
    <script src="src/module-1.js"></script>
    <script src="src/module-n.js"></script>
    ...
  </head>
  ...
</html>
{% endhighlight %}

You have to keep in mind to include the vendors first, so when the other modules that require them are loaded, their dependencies are met.

You also have to add manually each time a new file is created. It is easy to have a lot of small source files in a project if you want them properly decoupled.

A way of diminish the manual work required is to concatenate them in a single file and include it. [Gulp][gulp] has a plugin for that: [gulp-concat][concat]

[gulp]: http://gulpjs.com/
[concat]: https://www.npmjs.com/package/gulp-concat

{% highlight javascript %}
var concat = require('gulp-concat');

gulp.task('compile', function() {
  return gulp
    .src(['vendor/*.js', 'src/*.js'])
    .pipe(concat('my-project.js'))
    .pipe(gulp.dest('dist/'));
});
{% endhighlight %}
{% highlight html %}
<html>
  ...
  <head>
    <script src="dist/my-projet.js"></script>
  </head>
  ...
</html>
{% endhighlight %}

But the code will still need to be in a place that other modules can access it. And you don't want to start defining a lot of global variables because you will end up having name collisions at some point. The simplest thing that I have for now is a namespacing mechanism:

{% highlight javascript %}
if (typeof Namespace === 'undefined') {
  Namespace = {};
}
ModuleName.Module = //Your exposed module;
{% endhighlight %}

[Node][node] has this built in in the form of `require` and `module.exports = exposedModule`. But in a browser that is not available.

[node]: https://nodejs.org/

For that, I have been recommended [browserify][browserify]. I haven't tried it yet, but I will at some point if needed.

[browserify]: http://browserify.org/
