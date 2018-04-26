##	What is AngularJS?
AngularJS is a JavaScript framework used to create Single Page Application (SPA). It extends HTML DOM with new attributes.
##	What is SPA?
Single Page Applications are Web Apps or Websites that loads a single HTML page initially and then dynamically updates or loads parts of page on user interaction. They use AJAX to update page without reloading the page (without refreshing the browser).

##	What is deferred loading?
Loading HTML or JavaScript files through AJAX when needed.

## What is Data Binding?
AngularJS automatically synchronizes data between the model and view components (HTML elements). If user updates some value in view then model gets updated too and vice-versa this is known as Two-Way Data binding. We perform data binding by using directive ng-model or expressions.

##	What are Directives?
In AngularJS directives are attributes of HTML elements that extends their functionalities. For example ng-model directive when given to an input box it extends its functionality to bind its value to a model.

##	Explain Modules.
Module is a single core unit where we encapsulates our functionality. We write different controllers for related tasks and put them in a single module. A module is the main way to define an AngularJS application. It contains all of our application code. A single AngularJS app can contain several modules each pertains to specific functionality.
Some Advantages of Modules:
o	Allows to share code between applications.
o	Isolated functionality makes easier to test.
o	Keep global namespace clean.
o	Allow app to load code in any order.

##	Explain controllers.
Controllers are functions that manages the relationship between model and view. They augment the view of an AngularJS app. We write logic here to manage model objects.

##	What are Scopes?
Scopes are simple JavaScript plain object that serve as the glue between controllers and views. We can add and change properties on the $scope object as per our need. It is the data model.  When we create a controller, we pass a new $scope object to it. All the properties are automatically accessible to the view. Scopes are the source of truth for the application state because whenever view modifies $scope also update itself immediately and when $scope changes view also updates.

##	Explain $rootScope.
$rootScope object is the parent of all scopes. It acts like a global variable.

##	Explain Dependency Injection.
Dependency Injection is a design pattern that allows us to remove hard-coded dependencies for a component and make it possible to change and modify at run-time. This helps in making components reusable, maintainable and testable.
Let’s understand using a simple example:
function Component(externalComponent) {
    this.externalComponent = externalComponent;
}

Component.prototype.doSomething = function () {
    this.externalComponent.utilityFunction();
}
At runtime, the Component doesn’t care about how it gets externalComponent, so long as it gets it. The creator of the Component is responsible for passing in the Component dependencies when it’s created.
Angular uses $injector for managing lookups and instantiation of dependencies for all angular components including app modules, directives, controllers, etc.

##	Explain Digest Loop.
First we need to understand three terms to proceed:
Angular Context: It refers specifically to code that runs inside the Angular event loop referred to as the $digest loop. 
Dirty Checking: It is a simple process in which Angular compares a value with its previous value and if it has changed then a change event is fired.
Digest: It is a cycle that performs dirty checking.

There are two things that goes inside the Angular context:
1. $watch list
	For all UI elements that bound to a $scope object a $watch is added to the $watch list to update the view if property of $scope object is changed somehow.
In digest loop, Angular traverse the $watch list, and if the updated value has not changed	 from the old value, it continues to traverse the list. If the value has changed, the angular will records the new value and continues down the $watch list (dirty checking process). Once angular has traverse through the entire $watch list, if any value changed, the app will again fall back into the $watch loop until it detects that nothing has changed. And once the state is reached when there is no change in $watch list, $digest loop exists, and the browser repaints the DOM which refreshes the view.
2. $evalAsync list
	It is a way to schedule running expressions on the current scope at some time later in the future.

##	Difference between $apply() and $digest()
$digest():
It processes all of the watchers the current scope and its children. As we know watcher’s listener can the model, the $digest() keeps calling the watchers until no more listeners are firing. We don’t call $digest() directly in controllers or directives instead we should call $apply() which calls $digest().
$apply():
It is used to execute an expression in angular context from outside of the angular framework like browser DOM events, XHR, setTimeout, or libraries like jQuery, etc. One thing to remember that it executes the entire watchers in the application within every scope that we have.

•	Explain the usage of $watch and $watchCollection?
$watch():
It performs dirty checking on watchExpression(property of a scope object or a function) and registers a listener callback to be executed whenever the watchExpression changes.
$scope.watch('watchMe', function (newVal,olVal) {

    /*
        perform something
    */
})
$watchCollection():
It allows us to set shallow watches for object properties or elements of an array and fire the listener callback whenever the properties change.
$scope.$watchCollection('myCollection', function (newVals, oldVals, scope) {
   		 //collection has changed
});

##	How to implement routing in AngularJS.
In routing we break the whole application view into a layout and template views and only show the view we want to show based upon the URL the user is accessing currently.
angular.module('myApp', []).
config(['$routeProvider', function ($routeProvider) {
    $routeProvider
    .when('/', {
        templateUrl: 'views/home.html',
        controller: 'HomeController'
    })
    .when('/login', {
        templateUrl: 'views/login.html',
        controller: 'LoginController'
    })
    .when('/dashboard', {
        templateUrl: 'views/dashboard.html',
        controller: 'DashboardController',
        resolve: {
            user: function (SessionService) {
                return SessionService.getCurrentUser();
            }
        }
    })
    .otherwise({
        redirectTo: '/'
    });
}]);

##	Difference between service, factory and providers.
Services are singleton objects that are instantiated only once per app and created only when necessary. And after creation they can be shared across the application.
There are different methods for creating services, let’s see the major ones.

1. factory(): It’s the quick way to create and configure a service. It can return anything from a primitive value to a function to an object. Generally, when we create a factory, we create an object, add properties to it, then return that same object. And we access this factory in our controller using dependency injection, those properties on the object will be accessible in our controller.
app.factory('dataFactory', function () {

    var objFactory = {};
    objFactory.getData = function ()
    {
        return "data from factory";
    }
    return objFactory;
})

app.controller('myCTRL', function ($scope, dataFactory) {

    $scope.data = dataFactory.getData();
})

2. service(): In service, AngularJS instantiates it behind the curtain using a constructor function(using new keyword). Due to this we have add properties to ‘this’ and the service will return ‘this’. In controller we can access properties on ‘this’ using service.
app.service('dataService', function () {

    this.getData = function ()
    {
        return "data from service";
    }
})

app.controller('myCTRL', function ($scope, dataService) {

    $scope.data = dataService.getData();
})

3. provider(): A provider is an object with a $get() method. If we want to configure our service in config() method then we should use provider to create our service. It is he only service that can be passed into config() function. It helps us provide module-wise configuration for our service object before making it available.

##	Explain $http usage.
$http service is a function that takes a single argument called configuration object that is used to generate HTTP request. It returns a promise with two helper methods error and success. 
$http({
    method: 'GET',
    url: '/api/getdata'
}).success(function (data, status, headers, config) {
    // called when the response is
    // ready
}).error(function (data, status, headers, config) {
    // called when the response
    // comes back with an error status
});
##	Order of Call of methods in angularJS?
1.	app.config()
2.	app.run()
3.	Directive's compile functions if they are found in the DOM.
4.	app.controller()
5.           Directive’s link function

##	Where would you put your authentication logic?
run block
angular.module('yourApp').run(function ($rootScope, $location, AuthenticationService) {

  // enumerate routes that don't need authentication
  var routesThatDontRequireAuth = ['/login'];

  // check if current location matches route  
  var routeClean = function (route) {
    return _.find(routesThatDontRequireAuth,
      function (noAuthRoute) {
        return _.str.startsWith(route, noAuthRoute);
      });
  };

  $rootScope.$on('$routeChangeStart', function (event, next, current) {
    // if route requires auth and user is not logged in
    if (!routeClean($location.url()) && !AuthenticationService.isLoggedIn()) {
      // redirect back to login
      $location.path('/login');
    }
  });
});

##	Difference between $eval and $parse?
The important difference is that $eval is a scope method that executes an expression on the current scope, while $parse is a (more globally available) service.
So you can say $parse(expr)(context, locals);, with any context, but in the case of $eval the context will be the scope.
##	What can be injected in configuration block?
In configuration block, we can inject any provider ($httpProvider, $locationProvider, $compileProvider, etc). Services are not available in configuration block so we can inject services in configuration block.
##	Explain the usage of $broadcaset, $emit and $on?
$broadcast() as well as $emit() allow you to raise an event in your AngularJS application. The difference between $broadcast() and $emit() is that the former sends the event from the current controller to all of its child controllers. That means $broadcast() sends an even downwards from parent to child controllers. The $emit() method, on the other hand, does exactly opposite. It sends an event upwards from the current controller to all of its parent controllers. From the syntax point of view a typical call to $broadcast() and $emit() will look like this:

$scope.$broadcast("MyEvent",data);
$scope.$emit("MyEvent",data); 

Here, MyEvent is the developer defined name of the event that you wish to raise. The optional data parameter can be any type of data that you wish to pass when MyEvent is dispatched by the system. For example, if you wish to pas data from one controller to another that data can go as this second parameter.

An event raised by $broadcast() and $emit() can be handled by wiring an event handler using $on() method. A typical call to $on() will look like this:

$scope.$on("MyEvent", function(evt,data){ 
  // handler code here });



