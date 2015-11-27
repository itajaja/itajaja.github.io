---
layout: post
title: "A boilerplate for typescript and webpack"
modified:
excerpt: "A boilerplate project for using webpack, typescript and babel together"
tags: [typescript, webpack, es6, karma, mocha]
---

I always find that setting up a project's wire-frame of tools and scripts is the most daunting part of writing a Javascript application. The Unix-like philosophy of having small and composable tools has its weakest point in the integration among all the different parts. I was recently looking for a good way of using webpack as a build tool for typescript libraries, and I couldn't find a good boilerplate project that had **linting**, **testing** and **sourcemaps** and **typescript support** out of the box.

After reading [these](http://www.jbrantly.com/typescript-and-webpack/) [two](http://www.jbrantly.com/es6-modules-with-typescript-and-webpack/) very nice posts by James Brantly I decided it was worth spending some time to set up my boilerplate project. A lot of the information in this post is taken from those articles.

![](https://imgs.xkcd.com/comics/is_it_worth_the_time.png)

You can find the code on [github](https://github.com/itajaja/tslib-webpack-starter), issues and PRs are welcome.

## Compiling

I really like typescript the versatility and speed of webpack. Using a combination of [**ts-loader**](https://github.com/TypeStrong/ts-loader) and [**babel-loader**](https://github.com/babel/babel-loader) allows to build a library in one javascript file. Here are the important parts of `webpack.config.js`:

{% highlight javascript %}
output: {
  library: 'MyTsLibrary',
  path: BUILD_PATH,
  filename: 'index.js'
},

resolve: {
  extensions: ['', '.webpack.js', '.web.js', '.ts', '.tsx', '.js']
},

module: {
  loaders: [ { test: /\.tsx?$/, loader: 'babel!ts-loader' } ]
},
{% endhighlight %}

Typescript only provides the type checking, but the transpilation from ES6 to ES5 is carried over by babel, which is more webpack-friendly. This also allows to have some parts of the application written in js or jsx and have only babel run on them.

The starter comes with [**TSD**](http://definitelytyped.org/tsd/) installed to download all the type definition and keep the typescript compiler happy during imports.

simply run `npm run build` to bundle everything into one javascript file. This is suitable for library projects, but if you want your javascript files separate, just remove the `library` and `filename` properties from the `output` in the `webpack.config.js`.

## Linting

Using a linter decreases bikeshedding and increases maintainability. [**tslint**](https://github.com/palantir/tslint) is a eslint-like linter for typescript. The webpack loader, [**tslint-loader**](https://github.com/wbuchwalter/tslint-loader) makes it extremely easy to plug linting inside the build process:

{% highlight javascript %}
preLoaders: [ { test: /\.tsx$/, loader: "tslint" } ],
{% endhighlight %}

## Testing

Integrating [**karma**](http://karma-runner.github.io/0.13/index.html) and [**mocha**](https://mochajs.org/) with webpack is very easy. With [**karma-webpack**](https://github.com/webpack/karma-webpack) you can load the webpack build as a preprocessor of the karma task. [**chai**](http://chaijs.com/) is also installed for extra sugar.

## My Sublime Text

To take full advantage of typescript and the linting, I use Sublime Text 3 with the [**typescript**](https://github.com/Microsoft/Typescript-Sublime-plugin) and [**tslint**](https://github.com/lavrton/SublimeLinter-contrib-tslint) extension to have immediate feedback while I code. They are very easy to install and surprisingly fast. Give it a try.