#  Using NPM as a js runtime environment and GULP as an Asset Builder

There are other excellent package managers and javascript asset builders.  This process explains using npm and Gulp.

**Node.js** is an open-source, cross-platform JavaScript runtime environment for developing a diverse variety of tools and applications. Although Node.js is not a JavaScript framework, many of its basic modules are written in JavaScript. The runtime environment interprets JavaScript using Google's V8 JavaScript engine. **Npm** is a Node.js package manager and hence used to install node programs.  An **asset builder** is a framework to concatenate and minify or compress JavaScript and CSS assets and the chosen asset builder used here is Gulp.  **Gulp** is a JavaScript-based streaming build toolkit for client-side code. It can stream client-side files for triggered events in a build environment. Advantages of Gulp include the automation of common development tasks, the simplification of repetitive tasks, and a decrease in overall development time.

## Documentation and Terms
- [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Node](https://nodejs.org/en/)
- [Gulp](https://docs.asp.net/en/latest/client-side/using-gulp.html/)

##Getting started
- Run ```npm init ``` in the top level of the project. This **_creates a manifest_** file and npm will store the packages and the versions here which are needed for the project.

- Run ```npm install gulp -save-dev ``` to **_add gulp_**. This will __create the file *node_modules*__ and install the gulp package in it.  The **--save-dev** flag will save the gulp package to the manifest file, which is called package.json.  

- Make a **.gitignore** file in the top level of your project folder.  This is a list of folders and files will not be committed to your Git repository.

- Make **gulpfile.js** in the top level of our project directory.

##### Installing gulp files and requiring them in your code.

- Run ```npm install browserify --save-dev ``` to install the browserify package.  Browserify is responsible for adding keywords to translate the code into new JavaScript code that the browser understands. Before running gulp on a new machine for the first time, first install it globally using ```console npm install gulp -g ``` (_sudo npm install gulp -g if permission errors_).

- Run ```npm install vinyl-source-stream --save-dev ``` for packaging browserify and put the browserified source code into a new file (...and does more).

- Run ```npm install gulp-concat --save-dev``` for consolidating multiple JavaScript files into one file.

- Run ```npm install gulp-uglify --save-dev``` for minifiying the consolidated file.

- Run ```npm install gulp-util --save-dev``` to pass dependencies that allow for multiple build options.  For this project the environment variables are development build or a production build and gulp util has many more uses.

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
```
### Gitignore File
###### .gitignore
```file
node_modules/
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
## Downloading a project with node dependencies
Run ```npm install``` to reinstall packages. This will be needed everytime a github project that places certain files in their .gitignore file.  This is the only step needed to install all dependencies.
