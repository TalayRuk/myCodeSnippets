#  Java Script
## Documentation
- [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Node](https://nodejs.org/en/)
- [TypeScript](http://www.typescriptlang.org/)
- [Angular](https://angularjs.org/)

#### Terms:
 - npm : Node Package Manager


## Creating a program



Initialize a new npm in the top level of the project. This creates a manifest file which is where npm stores a list of packages and the versions needed for the project.  Add packages. Gulp will manage the other packages. When command is run it creates _node_modules_ and install the gulp package in it.  The **--save-dev** flag will save the gulp package to the "shopping list" in our manifest file, which is called package.json.  Rhe browserify package is responsible for adding keywords to translate the code into new JavaScript code that our browser does understand.  All packages should be included in the package.json file. The first time running gulp on a new machine install it on the system globally using **$ npm install gulp -g** (_$ sudo npm install gulp -g if permission errors_). **$ npm install** will reload the package for us.

###### console
```console
npm init
npm install gulp --save-dev 
npm install browserify --save-dev
npm install vinyl-source-stream --save-dev // This is 
```
###### .gitignore
```file
node_modules/
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
  return gulp.src(['./js/*-int.js'])
  .pipe(concat('allConcat.js'))
  .pipe(gulp.dest('./tmp'));
});

gulp.task('jsBrowserify',['concatInterface'], function() {
  return browserify({ entries: ['./tmp/allConcat.js'] })
    .bundle()
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
