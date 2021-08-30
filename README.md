# Browserify + Babelify + Gulp 

[Webpack](https://webpack.js.org/) is a wonderful tool for development when writing ReactJS. Alternatively you can write react code by using [Gulp](https://gulpjs.com/) with [Browserify](https://browserify.org/) and [Babelify](https://github.com/babel/babelify).  

First you need to install the modules:

```npm install --save-dev gulp babelify browserify vinyl-source-stream react react-dom @babel/core @babel/preset-env @babel/preset-react```


Assuming that the ``` gulpfile.js ``` is in the root of your project, you need to make the necessary changes for it to work properly.

```

const 
	gulp = require('gulp'),
 	browserify = require('browserify'),
    babelify = require('babelify'),
 	source = require('vinyl-source-stream'),
    js = "/your-js-folder";

function babelify() {

	return browserify(js + "app.jsx") // the input file
    .transform(babelify, { presets: ["@babel/preset-env", "@babel/preset-react"] })
    .bundle()
    .pipe(source('app.js')) // the output file
    .pipe(gulp.dest(js + "/js"));
}

exports.build = babelify; 

```

Open a terminal and run:

``` gulp build```

## + Live-Reloading

Say you want to browser to refresh everytime there is changes in your code, with [BrowserSync](https://browsersync.io/) you can setup a webserver which has the ability to live-reload among others stuffs like making accessible to any device on the same WiFi network.

First you need to install the module:

``` npm install --save-dev browser-sync ```


Update ``` gulpfile.js```:

```

const 
	gulp = require('gulp'),
    browserSync = require('browser-sync').create(), // add browser-sync 
 	browserify = require('browserify'),
    babelify = require('babelify'),
 	source = require('vinyl-source-stream'),
     
    js = "/your-js-folder";

function babelify() {

	return browserify(js + "app.jsx") // the input file
    .transform(babelify, { presets: ["@babel/preset-env", "@babel/preset-react"] })
    .bundle()
    .pipe(source('app.js')) // the output file
    .pipe(gulp.dest(js + "/js"));
}

function serve() {

    browserSync.init({
         baseDir: "./your-app-folder"
    });
    gulp.watch(js + "app.jsx", gulp.series( babelify, browserSync.reload) );
    
}

exports.default = serve;
exports.build = babelify; 

```






