#  Using NPM as a js runtime environment and GULP as an Asset Builder

There are other excellent package managers and asset builders.  **This process explains using npm, bower, and Gulp to locally host a server.**

**Node.js** is an open-source, cross-platform JavaScript runtime environment for developing a diverse variety of tools and applications. Although Node.js is not a JavaScript framework, many of its basic modules are written in JavaScript. The runtime environment interprets JavaScript using Google's V8 JavaScript engine. **Npm** is a Node.js package manager and hence used to install node programs.  An **asset builder** is a framework to concatenate and minify or compress JavaScript and CSS assets and the chosen asset builder used here is Gulp.  **Gulp** is a JavaScript-based streaming build toolkit for client-side code. It can stream client-side files for triggered events in a build environment. Advantages of Gulp include the automation of common development tasks, the simplification of repetitive tasks, and a decrease in overall development time.

## Documentation and Terms
- [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Node](https://nodejs.org/en/)
- [Gulp](https://docs.asp.net/en/latest/client-side/using-gulp.html/)
- [Bower](https://bower.io/)
- [Sass](http://sass-lang.com/)


## Getting started
- Run ```npm init ``` in the top level of the project. This **_creates a manifest_** file and npm will store the packages and the versions here which are needed for the project.

- Run ```npm install gulp -save-dev ``` to **_add gulp_**. This will __create the file *node_modules*__ and install the gulp package in it.  The **--save-dev** flag will save the gulp package to the manifest file, which is called package.json.  

- Make a **.gitignore** file in the top level of your project folder.  This is a list of folders and files will not be committed to your Git repository.

- Make **gulpfile.js** in the top level of our project directory.

##### Installing gulp files.

- Run ```npm install browserify --save-dev ``` to install the browserify package.  Browserify is responsible for adding keywords to translate the code into new JavaScript code that the browser understands. Before running gulp on a new machine for the first time, first install it globally using ``` npm install gulp -g ``` (_sudo npm install gulp -g if permission errors_).

- Run ```npm install vinyl-source-stream --save-dev ``` for packaging browserify and put the browserified source code into a new file (...and does more).

- Run ```npm install gulp-concat --save-dev``` for consolidating multiple JavaScript files into one file.

- Run ```npm install gulp-uglify --save-dev``` for minifiying the consolidated file.

- Run ```npm install gulp-util --save-dev``` to pass dependencies that allow for multiple build options.  For this project the environment variables are development build or a production build and gulp util has many more uses.

- Run ``` npm install jshint --save-dev ``` and ```npm install gulp-jshint --save-dev``` to install jshint.

##### Installing bower
- Run ```bower init ``` to install bower as another package manager.  It will be used here to manage front end files.  If needed, install bower globally ```npm install bower -g```

- To install regular libraries, run
```
bower install jquery --save
bower install bootstrap --save
bower install moment --save
```

##### Adding gulp files to the
- Run ```npm install bower-files --save-dev``` for allowing Gulp to streamline package deployment process.

- Run ```npm install browser-sync --save-dev``` to install BrowserSync to implement our development server with live reloading.

- Run ```npm install gulp-sass gulp-sourcemaps --save-dev ``` to intall Sass (optional).


### File Structure For Setup

###### filestructure

![Folder](img/folder.png "Folder") css  
-- ![Folder](img/file.png "Folder") styles.css  
![Folder](img/folder.png "Folder") img  
![Folder](img/folder.png "Folder") js  
-- ![Folder](img/file.png "Folder") project-logic  
-- ![Folder](img/file.png "Folder") project-interface  
![Folder](img/folder.png "Folder") scss  
-- ![Folder](img/file.png "Folder") styles.scss  
![Folder](img/file.png "Folder") .gitignore  
![Folder](img/file.png "Folder") .env  
![Folder](img/file.png "Folder") gulpfile.js  
![Folder](img/file.png "Folder") index.html  
![Folder](img/file.png "Folder") package.json  
![Folder](img/file.png "Folder") bower.json  


### Console Commands
###### console
```console
npm init
npm install gulp --save-dev
npm install browserify --save-dev
npm install vinyl-source-stream --save-dev
npm install gulp-concat --save-dev
npm install gulp-uglify --save-dev
npm install gulp-util --save-dev
npm install del --save-dev
npm install jshint --save-dev
npm install gulp-jshint --save-dev
bower init
bower install jquery --save
bower install bootstrap --save
bower install moment --save
npm install bower-files --save-dev
npm install browser-sync --save-dev
npm install gulp-sass gulp-sourcemaps --save-dev
```
or download all through installing (npm install, bower install) these json files:

### Json Files

###### package.json
```json
{
  "name": "ProjectName",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "bower-files": "^3.14.1",
    "browser-sync": "^2.17.2",
    "browserify": "^13.1.0",
    "del": "^2.2.2",
    "gulp": "^3.9.1",
    "gulp-concat": "^2.6.0",
    "gulp-jshint": "^2.0.1",
    "gulp-sass": "^2.3.2",
    "gulp-sourcemaps": "^2.0.1",
    "gulp-uglify": "^2.0.0",
    "gulp-util": "^3.0.7",
    "jshint": "^2.9.3",
    "vinyl-source-stream": "^1.1.0"
  }
}
```
###### bower.json
```json
{
  "name": "alarmclock",
  "description": "",
  "main": "index.js",
  "authors": [
    "Jonathan Buchner <websites@jonathanbuchner.com>"
  ],
  "license": "ISC",
  "homepage": "",
  "private": true,
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ],
  "dependencies": {
    "jquery": "^3.1.1",
    "bootstrap": "^3.3.7",
    "moment": "^2.15.1"
  }
}
```
### Gitignore File
###### .gitignore
```file
node_modules/
bower_components/
build/
temp/
.env
```
### Gulp File

This file provides and an example for the setup of an asset pipeline based on the above Gulp packages.  All interface files end with *-interfeace.js* and are included in the function with the concatInterface dependency.  All logic files must be exported and required by the corresponding interface file. Naming convention for exporting on logical files:
```js
exports.ConstructorModule = Constructor;
```
and naming convention for requiring interface file:
```js
var Constructor = require('./../js/projectname.js').ConstructorModule;
```

###### gulpfile.js
```js
var gulp = require('gulp');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var utilities = require('gulp-util');
var browserify = require('browserify');
var source = require('vinyl-source-stream');
var del = require('del');
var jshint = require('gulp-jshint');
var browserSync = require('browser-sync').create();
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');
var lib = require('bower-files')({
  "overrides":{
    "bootstrap" : {
      "main": [
        "less/bootstrap.less",
        "dist/css/bootstrap.css",
        "dist/js/bootstrap.js"
      ]
    }
  }
});
var buildProduction = utilities.env.production;

gulp.task("clean", function(){
  return del(['build', 'tmp']);
});

gulp.task('concatInterface', function(){
  return gulp.src(['./js/*-interface.js']) //This finds all the files
  .pipe(concat('allConcat.js'))
  .pipe(gulp.dest('./tmp'));
});

gulp.task('jsBrowserify',['concatInterface'], function() {
  return browserify({ entries: ['./tmp/allConcat.js'] })
    .bundle()  // bundle is part of the Browserify process.
    .pipe(source('app.js'))
    .pipe(gulp.dest('./build/js'));
});

gulp.task("minifyScripts", ["jsBrowserify"], function(){
  return gulp.src("./build/js/app.js")
    .pipe(uglify())
    .pipe(gulp.dest("./build/js"));
});

gulp.task('jshint', function(){
  return gulp.src(['js/*.js'])
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});

gulp.task('bowerJS', function () {
  return gulp.src(lib.ext('js').files)
    .pipe(concat('vendor.min.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./build/js'));
});

gulp.task('bowerCSS', function () {
  return gulp.src(lib.ext('css').files)
    .pipe(concat('vendor.css'))
    .pipe(gulp.dest('./build/css'));
});

gulp.task('bower', ['bowerJS', 'bowerCSS']);

//This is what builds everything else.
gulp.task("build", ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
  gulp.start('bower');
  gulp.start('cssBuild');
});
//Required reload files
gulp.task('jsBuild', ['jsBrowserify', 'jshint'], function(){
  browserSync.reload();
});

gulp.task('bowerBuild', ['bower'], function(){
  browserSync.reload();
});

gulp.task('htmlBuild', function() {
  browserSync.reload();
});

gulp.task('cssBuild', function() {
  return gulp.src(['scss/*.scss'])
    .pipe(sourcemaps.init())
    .pipe(sass())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('./build/css'))
    .pipe(browserSync.stream());
});

gulp.task('serve', function() {
  browserSync.init({
    server: {
      baseDir: "./",
      index: "index.html"
    }
  });
  gulp.watch(['js/*.js'], ['jsBuild']);
  gulp.watch(['bower.json'], ['bowerBuild']);
  gulp.watch(['*.html'], ['htmlBuild']);
  gulp.watch(["scss/*.scss"], ['cssBuild']);
});
```

### Html pages

##### index.html
```html
<html>
  <head>
    <link rel="stylesheet" href="build/css/scripts.css">
    <link rel="stylesheet" href="build/css/vendor.css">
    <script src="build/js/vendor.min.js"></script>
    <script type="text/javascript" src="build/js/app.js"></script>
    <title>Project Title</title>
  </head>
  <body>
    <h1 class="styles">For testing styles.css</h1>
    <h1>For testing bower packaged files like<small>boostrap</small></h1>
    <h1>For testing script.js.  <span class="click">Append Here:<span><span class="spot"></span></h1>
  </body>
</html>
```

## Downloading a project with node dependencies
Run ```npm install``` and ```bower install ``` to reinstall packages. As you can see from gulpfile.js, buld the project by running `gulp build` and `gulp serve`.
