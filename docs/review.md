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

# Installation/Usage Instructions in README (+)

As we have seen previously, the README is a great place to show the full story of how your app is used (from a user's perspective).  Include instructions for how to download and run the tests, run the app, e.g.:

```sh
$ git clone git@github.com:[USERNAME]/todo-challenge.git
$ cd todo-challenge
$ ???
$ ???
```

To run app:

```sh
$ ???
$ open ???
```

It's a great idea to include some screenshots in your README.  For more info on embedding images in a README: https://guides.github.com/features/mastering-markdown/

e.g.:
```
![Screenshot](https://path_to_your_image)
```

You will need to host your images somewhere, e.g.:
* http://imgur.com/
* http://dropbox.com/


## If using a framework, delete any unnecessary code

## Clean up

* Remove comments
* Try to avoid having log statements in your production code

## ensure package.json devDependencies lists bower as a dependency (++)

https://github.com/makersacademy/todo_challenge/issues/68 related?

# Step 3: Tests

## Tests with no expectation

## Tests with multiple expectation

try not to have multiple expectations in unit tests. you might want a different test for the error message

## No integration tests, or poor test coverage


# Step 4: Application code

## move data into controller/model where Possible

```javascript
var toDo = angular.module('ToDo', []);

//poluting the global name space further.. bad!
var defaultList = { "items":[
```

Prefer putting defaultList in the approrpriate controller

## Prefer list array outside of function

```javascript
toDo.controller('toDoController', [function() {
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

could be

```javascript
toDo.controller('toDoController', [function() {
  var self = this;
  if (self.list === undefined) {
    self.list = [];
  }

  self.addItem = function() {
    var item = {
      completed: false,
      task: self.toDoItem
    }
    self.list.push(item)
  };
```

## Over-reliance on online tutorials

## Not using controllerAs syntax

## Not using preferred array syntax for dependency injection (to avoid minify issues)

## Make use of available angular directives - e.g. checkboxes

## Make use of filters where appropriate

## Think about refactoring the todolist into its own service

## Let Angular update the interface rather than reaching in to the HTML:

Prefer

```javascript
self.submitForm = function(){
 self.taskToAdd = ''
}
```

over

```javascript
self.submitForm = function(){
  var form = document.getElementsByName('myForm')[0];
  form.reset();
  return false;
}
```

## Prefer ToDoController as todoCtrl syntax

```html
<body>
  <div ng-controller="ToDoController">
```

Prefer ToDoController as todoCtrl syntax. Note, you then have to put todoCtrl. before your viewmodels / bindings throughout the html.

http://toddmotto.com/digging-into-angulars-controller-as-syntax/

## Set up ng-model correctly

```html
<input type="text" name="newtodo" ng-model="newtodo" required />
```

should be ng-model="todoCtrl.newTodo"

## ???

```javascript
$scope.clearCompletedTasks = function(){
    $scope.todos = $scope.todos.filter(function(task){
      return !task.complete;
```

$scope.todos = is actually rewriting the view model every time you clear completed tasks - so the change is permanent.


### Prefer other Semantic HTML Elements to Divs Where Possible

It's tempting to wrap everything HTML5 in a div.  However we should try to make use of other semantic HTML elements where possible.  Try using this [flowchart](http://html5doctor.com/downloads/h5d-sectioning-flowchart.png).  This flowchart should help us choose item 2 from the following list

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

* http://html5doctor.com/lets-talk-about-semantics/
* http://learn.shayhowe.com/advanced-html-css/semantics-accessibility/ ?
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
