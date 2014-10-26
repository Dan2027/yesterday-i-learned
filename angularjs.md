* Installing AngularJS: `bower init && install --save angular#~1.2 angular-resource#~1.2` (among whatever else you plan to use)
* Creating an app: `app = angular.module('MyApp', ['ngResource'])`
* Attaching controllers to the app: `app.controller('controllerName', function ($thingsYouUse) {...})`
* Attaching filters (e.g. `{{ 'world' | greet }}`) to the app: `app.filter('greet', function() { return function(name) { return 'Hello, ' + name + '!'; }; })`
* Controllers can be nested; everything attached to the parent's `$scope` will also be available to child controllers within.