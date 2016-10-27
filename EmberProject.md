# Creating An Angular 2 Project
Last updated _10/27/16_ by _Jonathan_  

This is a summation of information for the [Epicodus Ember](https://www.learnhowtoprogram.com/javascript/ember-js) two week block, with additions from various documentation, for creating an ember project.

#### Create an ember project
+ Run `ember new [project-name]`
+ (optional) `ember install ember-bootstrap`  
+ Run `ember s`  
+ Visit your app at http://localhost:4200.

#### Start a cloned project
+ Run `npm install`  
+ Run `bower install`
+ Run `ember s`  
+ Visit your app at http://localhost:4200.

#### Folder Structure:
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

# Common commands and file structure

#### Common cli commands
`ember help generate` lists commands.  
`ember g route [routeName]` make a new route (js file, hbs file, and add route to router).  
`ember g model [modelName]` make new model (js file and js testfile).  
`ember g component [componentName]` create a new compononent (js file, hbs file, and testfile).  

_Remember: a dash is required in every component name to avoid possible naming conflicts with HTML elements._

## Views - hbs

### Common hbs syntax

### Helpers

###### The link, if, and loop helper  
```hbs
{{#link-to 'index'}} Home {{/link-to}}   
{{#if}}, {{else}} ,{{/if}}  
{{#each model as |object|}}, {{object.property}}'s, {{/each}}

{{input value=city id="city"}}
// Can grab on js side through city: this.get('city')    
```
_Remember: do no include spaces between your opening or closing Handlebars brackets.  It may lead to unexpected errors._

#### List of helpers
List of all "helpers" in the [EMBER.TEMPLATES.HELPERS](http://emberjs.com/api/classes/Ember.Templates.helpers.html) class.

#### Passing data down into a component and an action to the js file.
```hbs
{{new-component objects=model passedFunction = "activateFunction" }}
```

#### Activating an action handler in the js file.
```hbs
{{action 'actionName' argument argument }}>
```
By default, the {{action}} helper listens for click events. If we place it on a button, the action we denote will be automatically triggered when that button is clicked.

#### CookBook - A label
```html
<form>
  <div class="form-group">
    <label for="author">Author</label>
    {{input value=author id="author"}}
  </div>
  <button {{action 'add'}}>Add</button>
</form>
```
## Router
Adjusting for dynamic routing.
###### app/router.js
```js
this.route('rental', {path: '/rental/:rental_id'});
```
_The corresponding path is a template page for one object of a set._

## Model
Ember guide for [model relationships](https://guides.emberjs.com/v2.0.0/models/defining-models/#toc_one-to-one).  

Basic `property: DS.attr(),`  
For ONE-TO-ONE
+ `DS.belongsTo('profile')` linked with `DS.belongsTo('user')`  
For ONE-TO-MANY
+ `DS.belongsTo('post')` linked with `DS.hasMany('comment')`  
For MANY-TO-MANY  
+ `DS.hasMany('tag')` inked with `DS.hasMany('post')`

#### Example of a model
###### app/model/modelName
```js
import DS from 'ember-data';

export default DS.Model.extend({
  owner: DS.attr(),
  city: DS.attr()
});
```

#### Example of two models with one to many relationships
###### app/models/post.js
```js
import DS from 'ember-data';

export default DS.Model.extend({
  comments: DS.hasMany('comment')
});
```
###### app/models/comment.js
```js
import DS from 'ember-data';

export default DS.Model.extend({
  post: DS.belongsTo('post')
});
```

## Route Handler

### Route Handler - Retrieving data into the model

###### Returning objects contained within the route handler

```js
import Ember from 'ember';

var rentals = [{
  id: 1,
  owner: "Veruca Salt",
  city: "San Francisco",
  type: "Estate",
  bedrooms: 15,
  image: "https://upload.wikimedia.org/wikipedia/commons/c/cb/Crane_estate_(5).jpg"
}, {
  id: 2,
  owner: "Mike TV",
  city: "Seattle",
  type: "Condo",
  bedrooms: 1,
  image: "https://upload.wikimedia.org/wikipedia/commons/0/0e/Alfonso_13_Highrise_Tegucigalpa.jpg"
}];

export default Ember.Route.extend({
  model() {
    return rentals;
  }
```

###### Getting a set of objects from firebase
```js
model() {
  return this.store.findAll('object');
},
```

###### Getting one object from a single Model from firebase
This is used in part with dynamic routing.  Params.rental_id is passed by the router.  The coressponds hbs page is designed to only accept a single object from the model.
```js
model(params) {
  return this.store.findRecord('object', params.rental_id);
},
```

###### Getting All Objects From Multiple Models from firebase
```js
model() {
  return Ember.RSVP.hash({
    rental: this.store.findAll('object1'),
    review: this.store.findAll('object2')
  });
```
_Ember.RSVP.hash is waiting for rental and review to load before passing information. It's waiting for the other promises and then returning the data in one promise._

### Route Handler - Methods for interacting with a DataStore
### Ember methods
```js
//Special Ember Commands for working with a datastore
  object.destroyRecord();  // Delete a record.
  object.save();   //Save a record.  We also use this to update a record by saving over parts.
  .createRecord() // For creating an object before ssaving
  object.addObject() //
  this.transitionTo('rental');  //Used in every method in a route handler.
  this.transitionTo('rental', rental.id);  //Dynamic transition.
```
### JavaScript methods needed for more complex interaction

#### Attached to an array.

###### .find()
The find() method returns a value of the first element in the array that satisfies the provided testing function. Otherwise undefined is returned.
```js
arr.find(callback[, thisArg])

function isBigEnough(element) {return element >= 15;}
[12, 5, 8, 130, 44].find(isBigEnough); // 130
```    

###### .map()
The map() method creates a new array with the results of calling a provided function on every element in this array.
```js
new_array = arr.map(callback[, thisArg])

var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt);
//roots is now [1, 2, 3]
//numbers is still [1, 4, 9]
```  

#### Attached to a map object.

###### .get()
The get() method returns a specified element from a Map object.
```js
Syntax myMap.get(key);

var myMap = new Map();
myMap.set("bar", "foo");
myMap.get("bar");  // Returns "foo".
myMap.get("baz");  // Returns undefined.
```

###### .set()
The set() method adds or updates an element with a specified key and value to a Map object.
```js
myMap.set(key, value);

// Add new elements to the map
myMap.set("bar", "foo");
myMap.set(1, "foobar");
// Update an element in the map
myMap.set("bar", "baz");
```        
### Route Handler -  Cook Book Action Handlers

###### Adding a simple action
```js
deleteReview(object) {
    object.destroyRecord();
    this.transitionTo('rental');
  }
```

###### Adding a save action
```js
save(params) {
  var newObject=this.store.createRecord('object',params);
  newObject.save();
  this.transitionTo('index');
}
```

###### How to write an update function
```js
update(object, params) {    //Params are grabbed at the component level, usually from inside the argument
  Object.keys(params).forEach(function(key){
    if(params[key]!==undefined) {
      object.set(key,params[key]);
    }
  });
  object.save();
  this.transitionTo('dynamicRoute', object.id);
},
```
###### Writing add function for connecting with child
```js
addReview(params) {
  var newReview=this.store.createRecord('review',params);
  var rental = params.rental;
  rental.get('reviews').addObject(newReview);
  newReview.save().then(function() {
    return rental.save();
  });
  this.transitionTo('rental', rental);
},

add(params) {
  var waitAdd=this.store.createRecord('parent',params);
  var parent = params.parent;    //passed in params originally
  parent.get('parents').addObject(waitAdd);
  newReview.save().then(function() {
    return rental.save();
  });
  this.transitionTo('rental', rental);
}
```

###### Writing a delete function to also delete child object relationship.
```js
deleteRental(rental) {
  var review_deletions = rental.get('reviews').map(function(review) {
    return review.destroyRecord();
  });
  Ember.RSVP.all(review_deletions).then(function() {
    return rental.destroyRecord();
  });
  this.transitionTo('index');
},

delete(parentObject) {
  var deleteWait = parentObject.get('linkProperty').map(function(child) {
    return child.destroyRecord();
  });
  Ember.RSVP.all(deleteWait).then(function() {
    return parentObject.destroyRecord();
  });
  this.transitionTo('index');
}
```
## Components - JS side

###### Sending up an function with an argument
```js
thingToShow: false,
actions: {
  show(){
    this.set('thingToShow', true);
  }
}    
```

###### Sending up an function with an argument
_Remember: It's important that the variable names we assign these values match the rental model’s attribute names._
```js
funtionToPass(){
  var params = {
    author: this.get('author'),
  };
  this.sendAction('funtionToPass', params);
}
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
+ Constant: Constants are block-scoped, much like variables defined using the let statement. The value of a constant cannot change through re-assignment, and it can't be redeclared.
+ Debugger: `debugger` The debugger statement stops the execution of JavaScript, and calls (if available) the debugging function.  Using the debugger statement has the same function as setting a breakpoint in the code.

#### Handle Bars
Handlebars provides the power necessary to let you build semantic templates effectively with no frustration.  [handlebarsjs.com/](http://handlebarsjs.com/)Handlebars Page.

#### Ember
+ **Dynamic segment:** A dynamic segment is simply a placeholder that may be dynamically updated depending on the circumstances (ie: which link we click to travel to this route). In our case, the dynamic segment we add to the rental route will represent the id of a given rental in Firebase. Including this specific information will allow us to return only a single rental in the route's model hook.
+ **Hook:** In the Ember.js framework, this refers to a method within an Ember class.
+ **Model Hook:** A method added to a route handler responsible for returning model data to that route, and its corresponding template. A model hook looks something like this:
+ **Route Handler:** Renders a template and loads a model that is then available to that template.
+ **Model:** Represent persistent state, and typically persist information to a web server. Models outline the attributes of objects.
+ **Components:** Consists of two files: A template written in Handlebars, and a source file written in JavaScript. The files have the same name, but the template ends in the Handlebars .hbs extension, and the source file ends in the .js extension. The template defines what the component should look like and the source file controls what functionality that component has. Components are called from within templates, or within the .hbs portion of another component.
+ **Template:** Define how information is displayed, and organize the layout of HTML in an application. Ember templates use the syntax of Handlebars templates. Anything that is valid Handlebars syntax is valid Ember syntax. Templates can also display properties provided to them from their context, which is either a component or a route.
+ **Action Name:**   {{action actionname argument1 argument2}} - The actionName is in the middle.
+ **Action Handler:** the corresponding function to the action name.

#### Important Ember Classes

+ [EMBER.MAP CLASS](http://emberjs.com/api/classes/Ember.Map.html#content) A Map stores values indexed by keys. Unlike JavaScript's default Objects, the keys of a Map can be any JavaScript object.
+ [.extend](http://emberjs.com/api/classes/Ember.CoreObject.html#method_extend)  - You can also create a subclass from any existing class by calling its extend() method.
