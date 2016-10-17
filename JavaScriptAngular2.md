# Creating An Angular 2 Project
Angular is a JavaScript framework written to take advantage of TypeScript. There is an API that allows using standard ECMAScript 5 JavaScript, it is recommended to use TypeScript.  The design of this project template is dicrectly from the Angular 2 Documentation.

Notes:
+

### Documentation
+ [Angular 2](https://angular.io/docs/ts/latest/quickstart.html)
+ [TypeScrpit - _link broken_](https://angular.io/docs/ts/latest/quickstart.html)

### Starting Files

###### file structure

![Folder](img/folder.png "Folder") resources   
-- ![Folder](img/folder.png "Folder") styles  
-- ![Folder](img/folder.png "Folder") js  
-- ![Folder](img/folder.png "Folder") images   
-- ![Folder](img/folder.png "Folder") app  
-- --       ![File](img/file.png "file") app.component.ts  
![File](img/file.png "file") index.html  
![File](img/file.png "file") package.json  
![File](img/file.png "file") .gitignore  

###### index.html

```html
<html>
  <head>
    <title>Angular 2 Skeleton</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <script src="build/js/vendor.min.js"></script>
    <link rel="stylesheet" href="build/css/vendor.css">
    <!-- 1. Load libraries -->
     <!-- Polyfill(s) for older browsers -->
    <script src="node_modules/core-js/client/shim.min.js"></script>
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <!-- 2. Configure SystemJS -->
    <script src="systemjs.config.js"></script>

    <link rel="stylesheet" href="build/css/styles.css">
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
  </head>
  <!-- 3. Display the application -->
  <body>
    <my-app>Loading...</my-app>
  </body>
</html>
```

###### package.json
```json
{
  "name": "angular2-skeleton",
  "version": "1.0.0",
  "scripts": {
    "start": "tsc && concurrently \"npm run tsc:w\" \"npm run lite\" ",
    "lite": "lite-server",
    "postinstall": "typings install",
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "typings": "typings"
  },
  "license": "ISC",
  "dependencies": {
    "@angular/common": "2.0.0-rc.6",
    "@angular/compiler": "2.0.0-rc.6",
    "@angular/compiler-cli": "0.6.0",
    "@angular/core": "2.0.0-rc.6",
    "@angular/forms": "2.0.0-rc.6",
    "@angular/http": "2.0.0-rc.6",
    "@angular/platform-browser": "2.0.0-rc.6",
    "@angular/platform-browser-dynamic": "2.0.0-rc.6",
    "@angular/router": "3.0.0-rc.2",
    "@angular/upgrade": "2.0.0-rc.6",
    "core-js": "^2.4.1",
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.0-beta.11",
    "systemjs": "0.19.27",
    "zone.js": "^0.6.17",
    "angular2-in-memory-web-api": "0.0.18",
    "bootstrap": "^3.3.6"
  },
  "devDependencies": {
    "bower-files": "^3.11.3",
    "browser-sync": "^2.11.1",
    "del": "^2.2.0",
    "gulp": "^3.9.1",
    "gulp-concat": "^2.6.0",
    "gulp-sass": "^2.2.0",
    "gulp-shell": "^0.5.2",
    "gulp-sourcemaps": "1.6.0",
    "gulp-uglify": "^1.5.3",
    "gulp-util": "^3.0.7",
    "concurrently": "^2.2.0",
    "lite-server": "^2.2.2",
    "typescript": "^1.8.10",
    "typings":"^1.3.2"
  }
}
```

###### .gitignore
```
node_modules/
npm-debug.log
bower_components/
app/*.js
app/*.js.map
.DS_Store
build/
typings/
```

### Definitions
**Components:** are the basic building blocks of an Angular 2 app.  A _component_ has a selector property. **Selectors** interact with DOM elements, the _component's selector is the DOM element the component is attached to_ (generally a new HTML tag). A _component_ also must include a template. A **template** is made up of the HTML that we want to display inside of our _selector_.  A _component_ has two halves: An annotation and a class definition.

###### example
```ts
@Component({
  selector: 'my-app',
  template: `
  <h1>My First Angular 2 App</h1>
  `
})
export class AppComponent {
}
```
