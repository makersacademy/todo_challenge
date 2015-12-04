# Introduction

Welcome to the code review for the Todo Challenge!  Again, don't worry - you are not expected to have all the answers. The following is a code-review scaffold for Todo Challenge that you can follow if you want to.  These are common issues to look out for in this challenge - but you may decide to take your own route.

If you can please use this form to tick off where your reviewee has successfully has successfully incorporated these guidelines!  This form helps us get an overall picture of how the whole cohort is doing - it's not an assessment of an individual student.

# Step 0: Checkout and Run tests

Please checkout your reviewee's code and run their tests. Read the code and try and use the app through the web interface.  You can also experiment with the engine in the console. How easy is it to understand the structure of their code? How readable is their code? Did you need to make any cognitive leaps to 'get it'?  Is it more complicated than it needs to be?

# Step 1:  How far did they get?

* Features
  * [ ] Can store tasks
  * [ ] Instant updates
  * [ ] Marking tasks as done

* Bonus Features
  * [ ] Filters
  * [ ] Display number of tasks
  * [ ] Clearing completed tasks
  * [ ] Deploy the app
  * [ ] Persistence layer with Node
  * [ ] CSS Awesomeness

# Step 2: Structure and supporting files

## Installation/Usage Instructions in README

As we have seen previously, the README is a great place to show the full story of how your app is used (from a user's perspective).  Include instructions for how to download and run the tests, run the app, e.g.:

To download the app and run tests e.g.:

```sh
$ git clone git@github.com:[USERNAME]/todo-challenge.git
$ cd todo-challenge
$ npm install
$ bower install

```

To run unit tests e.g.:

```sh
$ npm test
```

To run feature tests e.g.:

```sh
$ webdriver-manager start  # in separate console
$ protractor test/e2e/conf.js
```

To run app e.g.:

```sh
$ http-server .
```

etc.

It's a great idea to include some screenshots in your README.  For more info on embedding images in a README: https://guides.github.com/features/mastering-markdown/

e.g.:
```
![Screenshot](https://path_to_your_image)
```

You will need to host your images somewhere, e.g.:
* http://imgur.com/
* http://dropbox.com/


## If using a framework such as Yeoman to scaffold your solution, delete any superfluous code

Some frameworks create lots of additional cruft to support features you are not using.  Delete these from your solution so that they do not distract your readers.  If you are worried about needing them in the future, you can always keep them on a branch.

## Clean up

* Remove commented out code and superfluous commentary
* Avoid having `console.log` statements in your production code
* Ensure you do not commit your bower_components and node_modules folders to git

## Ensure project setup is reproducible on a different machine

For example, ensure all of you node dependencies are correctly saved in `package.json` and that bower dependencies are saved in `bower.json`.

# Step 3: Tests

## Tests with no expectation

Don't forget to include expectations in your tests.  For example the below has a long setup, but no expectation

```javascript
it('allows a task to be deleted', function() {
  ctrl.toDoText = 'Test todo';
  ctrl.addToDo();
  ctrl.deleteItem(0);
});
```

## Tests with multiple expectation

Avoid multiple expectations in unit tests.  Create a new unit test for each expectation.

## No end to end tests

Use protractor to ensure your application is fully feature tested.

# Step 4: Application code

## Encapsulate data in the controller or service

e.g. rather than:

```javascript
// defaultList is declared in a global scope
var defaultList = { "items": [ ] }

todoApp.controller('ToDoController', function() {
  // and then its data is mutated within a controller
  this.addItem = function(item) {
    defaultList.items.push(item)
  }
});
```

declare `defaultList` within the controller:

```javascript
todoApp.controller('ToDoController', function() {
  var defaultList = { "items": [ ] }

  this.addItem = function(item) {
    defaultList.items.push(item)
  }
});
```

## Initialize state outside of interface functions

In the example below `addItem` additionally tests for and initializes `self.list`.  This is not its responsibility and necessitates a redundant test on all but the first call to the function.

```javascript
toDo.controller('ToDoController', [function() {
  var self = this;

  self.addItem = function() {
    if (self.list === undefined) {
      self.list = [];
    }
    var item = {
      completed: false,
      task: self.toDoItem
    }
    self.list.push(item)
  };
```

Instead, this should be:

```javascript
toDo.controller('ToDoController', [function() {
  var self = this;

  self.list = [];

  self.addItem = function() {
    var item = {
      completed: false,
      task: self.toDoItem
    }
    self.list.push(item)
  };
```

## Over-reliance on online tutorials

Beware relying on code from older resources: it may use deprecated conventions.  Also, do not copy-paste you don't fully understand.

## Not using controllerAs syntax

The following syntax is valid:

```html
<div ng-controller="ToDoController">
```

however, the ['controllerAs' syntax is preferred](https://github.com/toddmotto/angularjs-styleguide#controllers):

```html
<div ng-controller="ToDoController as todoCtrl">
```

## Use preferred array syntax for dependency injection (to avoid minify issues)

Don't define your Angular components as follows:

```javascript
module.controller(function($dependency1, $dependency2, ...) {
  ...
});
```

If this code is minified, the two parameters `$dependency1` and `$dependency2` will be renamed with short names such as `a` and `b`.  Angular will not be able to resolve these names as dependencies.  To prevent this, define your components using the array syntax:

```javascript
module.controller("$dependency1", "$dependency2", function($dependency1, $dependency2, ...) {
  ...
});
```

## Make use of available Angular directives - e.g. checkboxes

Angular has many awesome directives to take care of the common UI requirements.  Use them - that's what they're for!  If you can't find a directive and think there should be one... write it!

## Make use of filters where appropriate

https://docs.angularjs.org/guide/filter

## Think about creating a service to manage the todo list data

In Angular, services are []**singletons**](https://en.wikipedia.org/wiki/Singleton_pattern), whereas controllers are not.  If you need two instances of your controller on the page, you will have two sets of data (if the data is contained in the controller).  However, if you store the data in a [**service**](https://docs.angularjs.org/guide/services) and inject this service into your controller, each controller instance will access the same single instance of the service, so there will be only one set of data.

## Let Angular update the interface rather than reaching into the HTML:

For example, prefer:

```javascript
self.submitForm = function(){
 self.taskToAdd = ''
}
```

over:

```javascript
self.submitForm = function(){
  var form = document.getElementsByName('myForm')[0];
  form.reset();
  return false;
}
```

In particular, if you are directly manipulating UI elements in response to changes in application state, you are probably doing something wrong and should be using Angular instead.

## Set up ng-model correctly with controllerAs syntax.

```html
<div ng-controller="ToDoController as todoCtrl">
  <input type="text" name="newTodo" ng-model="newTodo" required />
</div>
```

should be:

```html
<div ng-controller="ToDoController as todoCtrl">
  <input type="text" name="newTodo" ng-model="todoCtrl.newTodo" required />
</div>
```

## Don't reference `$scope`

When using the `controllerAs` syntax, define your controller's behaviour on `this` instead of injecting `$scope` (`this` is already bound to `$scope`):

avoid:

```javascript
function MainCtrl ($scope) {
  $scope.someObject = {};
  $scope.doSomething = function () {

  };
}
```

recommended:

```javascript
function MainCtrl () {
  var self = this;
  self.someObject = {};

  self.doSomething = function () {
    self.someObject.someProperty;
  };
}
```

Note: the use of `self = this;` at the top of the function is important.  The following _will not work_ as `this` inside `doSomething` is not bound to `$scope`:

```javascript
function MainCtrl () {
  this.someObject = {};

  this.doSomething = function () {
    this.someObject.someProperty;
  };
}
```

## Prefer semantic HTML elements to `div`s where possible

It's tempting to wrap everything in a div.  However we should try to make use of other semantic HTML elements where possible.  Try using this [flowchart](http://html5doctor.com/downloads/h5d-sectioning-flowchart.png).  This flowchart should help us choose item 2 from the following list

* 1. Pure Div

```html
<div class='comment'>
</div>
```

* 2. Article (HTML5 recommended)

```html
<article class='comment'>
</article>
```

* 3. Creating your own HTML5 element (avoid unless really required)

```html
<comment>
</comment>
```

Related links:

* http://html5doctor.com/lets-talk-about-semantics
* http://learn.shayhowe.com/advanced-html-css/semantics-accessibility
* http://www.w3schools.com/html/html5_semantic_elements.asp

## Extract CSS to separate files:

Avoid embedding your styles in HTML files like so:

```html
<style>
  .complete{text-decoration: line-through;color:#ccc;}
</style>
```

Prefer moving them to a file such as `my_styles.css`:

```css
  .complete{
    text-decoration: line-through;
    color:#ccc;
  }
```

and then reference them from your HTML like so:

```html
<link rel="stylesheet" href="my_styles.css">
```
