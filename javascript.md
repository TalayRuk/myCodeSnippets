#  JavaScript
## Documentation and Terms
- [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Node](https://nodejs.org/en/)
- [TypeScript](http://www.typescriptlang.org/)
- [Angular](https://angularjs.org/)

## Using NPM and GULP

**Node.js** is an open-source, cross-platform JavaScript runtime environment for developing a diverse variety of tools and applications. Although Node.js is not a JavaScript framework,[3] many of its basic modules are written in JavaScript, and developers can write new modules in JavaScript. The runtime environment interprets JavaScript using Google's V8 JavaScript engine. **Npm** is a NodeJS package manager: you can use it to install node programs.  The chosen **asset builder**--providing a framework to concatenate and minify or compress JavaScript and CSS assets--is Gulp.  **Gulp** is a JavaScript-based streaming build toolkit for client-side code. It is commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment. Some advantages of using Gulp include the automation of common development tasks, the simplification of repetitive tasks, and a decrease in overall development time.

##### Getting started
- Run ```console npm init ``` to initialize in the top level of the project. This _creates a manifest_ file which is where npm stores a list of packages and the versions needed for the project.

- Run ```console npm install gulp ``` to _add gulp_. When command is run it creates file ```node_modules``` and install the gulp package in it.  The **--save-dev** flag will save the gulp package to the manifest file, which is called package.json.  

- Make a **.gitignore** file in the top level of your project folder.  This is a list of folders and files that are inside of your project directory which you don't want to commit to your Git repository.

- Make **gulpfile.js** in the top level of our project directory.

##### Installing gulp files and requiring them in your code.

- Run ```npm install browserify --save-dev ``` to install the browserify package.  Browserify is responsible for adding keywords to translate the code into new JavaScript code that our browser understands. The first time running gulp on a new machine install it on the system globally using ```console npm install gulp -g ``` (_sudo npm install gulp -g if permission errors_).

- Run ```npm install vinyl-source-stream --save-dev ``` to package browserify and put the browserified source code into a new file (...and does more).

- Run ```npm install gulp-concat --save-dev``` to consolidating multiple JavaScript files into one file.

- Run ```npm install gulp-uglify --save-dev``` for minifiying the consolidated file.

- Run ```npm install gulp-util --save-dev``` (for this settup) to pass dependencies that allow for multiple build options.  For this project the environment variables are development build or a production build.

##### After cloning back down a
Run ```npm install``` to reintall packages.

### Console Commands
###### console
```console
npm init
npm install gulp --save-dev
npm install browserify --save-dev
npm install vinyl-source-stream --save-dev // This is
npm install gulp-concat --save-dev
npm install gulp-uglify --save-dev
npm install gulp-util --save-dev
$ npm install del --save-dev
```
### Gitignore File
###### .gitignore
```file
node_modules/
```
### Gulp File

This file provides and an example for the setup of an asset pipeline based on the above Gulp packages.  All interface files end with *-interfeace.js* and are included in the function with the concatInterface dependency.  All logic files must be exported and required by the corresponding interface file. Naming convention for exporting:
```js
exports.ConstructorModule = Constructor;
```
and naming convention on interface file:
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
var buildProduction = utilities.env.production;

gulp.task('concatInterface', function(){
  return gulp.src(['./js/*-interfeace.js']) //This finds all the files
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

gulp.task("clean", function(){
  return del(['build', 'tmp']);
});

gulp.task("build", ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
});
```
