# Creating An Angular 2 Project
Last updated _10/27/16_ by _Jonathan_  

This is a summation of information for the [Epicodus Ember](https://www.learnhowtoprogram.com/javascript/ember-js) two week block, with additions from various documentation, for creating an ember project.

## Create an Ember Project
`ember new [project-name]`

##### Folder Structure:
+ **app:** This is where folders and files for models, components, routes, templates and styles are stored. The vast majority of our coding happens in this folder.
+ **bower-components / bower.json:** bower is a dependency management tool. It is used to manage front-end plugins and component dependencies (HTML, CSS, JavaScript, etc). All bower components are installed in the bower-components directory. If we open bower.json, we see the list of dependencies that are installed by Ember automatically. These include Ember, jQuery, Ember Data and QUnit (for testing). If we add additional front-end dependencies, like bootstrap, they will also be listed here and their files added to the bower-components directory.
+ **config:** Contains the environment.js file which lists environmental settings and configurations for our app.
+ **dist:** When an app is built for deployment, the output files will reside here.
+ **node_modules / package.json:** This directory and file are from npm. npm is the package manager for Node.js. Ember is built with Node and uses a variety of Node.js modules for operation. The package.json file comes pre-loaded with a list of current packages Ember requires. Any Ember-CLI add-ons you install will also show up here. Packages listed in package.json are installed in the node_modules directory.
+ **public:** This directory will contain assets such as images and fonts.
+ **vendor:** This directory contains any front-end dependencies NOT managed by bower.
+ **tests / testem.json:** Contains automated tests for our app. Ember CLI's test runner testem is also configured in testem.json.
+ **tmp:** Temporary files live here.
+ **ember-cli-build.js:** Behind the scenes, Ember CLI uses a tool called Broccoli to compile our code. This file contains settings for how Broccoli should build our app.

##### Install ember dependencies
`npm install`  
`bower install`

##### Start the server
`ember s`  
Visit your app at http://localhost:4200.

## Common commands and file structure

#### Common hbs syntax
`{{#link-to 'index'}} Home {{/link-to}}`   
`{{#if}}`, `{{else}}` ,`{{/if}}`
`{{#each model as |object|}}`, `{{object.property}}'s`, `{{/each}}`   

_**Remember:** Do no include spaces between your opening or closing Handlebars brackets.  It may lead to unexpected errors._

#### Common cli commands
`ember help generate` lists commands.  
`ember g route [routeName]` make a new route (js file, hbs file, and add route to router).  
`ember g [modelName]` make new model (js file and js testfile).  
`ember g component rental-tile` create a new compononent (js file, hbs file, and testfile). **A dash is required in every component name to avoid possible naming conflicts with HTML elements**

#### Model File: _app/model/modelName_
```js
import DS from 'ember-data';

export default DS.Model.extend({
  owner: DS.attr(),
  city: DS.attr(),
  type: DS.attr(),
  image: DS.attr(),
  bedrooms: DS.attr()
});
```

## Using Firebase with Ember data
[Set up an account.](https://firebase.google.com/)  Run `ember install emberfire`.  This will create app/adapters/application.js.  Remember **table names in Firebase will be a plural model name, and the model hooks in your routes will refer to the singular model name.**  

#### Adjust config/environment.js to connect to firebase.
###### config/environment.js
```js
EmberENV: {
  FEATURES: {
    // Here you can enable experimental features on an ember canary build
    // e.g. 'with-controller': true
  }
},

firebase: {
  apiKey: 'YOUR-API-KEY-HERE',
  authDomain: 'YOUR-FIREBASE-APP.firebaseapp.com',
  databaseURL: 'https://YOUR-FIREBASE-APP.firebaseio.com',
  storageBucket: 'YOUR-FIREBASE-APP.appspot.com'
},

APP: {
```

#### Altering Firebase Permissions
###### _Realtime Database Rules on firebase_
```js
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

##### Update Model Hook:
```js
import Ember from 'ember';

export default Ember.Route.extend({
  model() {
    return this.store.findAll('rental');
  },
});
```
## Terms

#### General
+ Firebase: Is a cloud database maintained by Google that stores information in JSON format. Visit Firebase's website for more details.
+ Add-On: Code that extends a framework's functionality but is not part of its core codebase.
+ Adapters: Code that connects an application to its data store.
+ Seeding: Pre-loading the database with hard-coded data.
+ JSON: Stands for JavaScript Object Notation and is a standard format for communicating data between systems.

#### JavaScript
+ Constant : Constants are block-scoped, much like variables defined using the let statement. The value of a constant cannot change through re-assignment, and it can't be redeclared.

#### Handle Bars
[handlebarsjs.com/](http://handlebarsjs.com/)Handlebars Page

+ Handlebars provides the power necessary to let you build semantic templates effectively with no frustration.

#### Ember
+ **Hook:** In the Ember.js framework, this refers to a method within an Ember class.
+ **Model Hook:** A method added to a route handler responsible for returning model data to that route, and its corresponding template. A model hook looks something like this:
+ **Route Handler:** Renders a template and loads a model that is then available to that template.
+ **Model:** Represent persistent state, and typically persist information to a web server. Models outline the attributes of objects.
+ **Components:** Consists of two files: A template written in Handlebars, and a source file written in JavaScript. The files have the same name, but the template ends in the Handlebars .hbs extension, and the source file ends in the .js extension. The template defines what the component should look like and the source file controls what functionality that component has. Components are called from within templates, or within the .hbs portion of another component.
+ **Template:** Define how information is displayed, and organize the layout of HTML in an application. Ember templates use the syntax of Handlebars templates. Anything that is valid Handlebars syntax is valid Ember syntax. Templates can also display properties provided to them from their context, which is either a component or a route.

#### Important Ember Methods

+ [EMBER.MAP CLASS](http://emberjs.com/api/classes/Ember.Map.html#content) A Map stores values indexed by keys. Unlike JavaScript's default Objects, the keys of a Map can be any JavaScript object.
+ [.extend](http://emberjs.com/api/classes/Ember.CoreObject.html#method_extend)  - You can also create a subclass from any existing class by calling its extend() method.
