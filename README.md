# StudyMaterial
## Angular
### Angular Module
The basic building blocks of an Angular application are NgModules, which provide a compilation context for components. NgModules collect related code into functional sets; an Angular app is defined by a set of NgModules. An app always has at least a root module that enables bootstrapping, and typically has many more feature modules
Every Angular app has a root module, conventionally named AppModule, which provides the bootstrap mechanism that launches the application. An app typically contains many functional modules.


### NgModules:
    - Helps organise an application into cohesive blocks of functionality
    - NgModules can import functionality from other NgModules, and allow their own functionality to be exported and used by other NgModules.
    - An NgModule is defined as a class decorated with @NgModule. The @NgModule decorator is a function that takes a single metadata object whose properties describe the module. The most important properties are as follows.
        declarations — The components, directives, and pipes that belong to this NgModule.
        exports — The subset of declarations that should be visible and usable in the component templates of other NgModules.
        imports — Other modules whose exported classes are needed by component templates declared in this NgModule.
        providers — Creators of services that this NgModule contributes to the global collection of services; they become accessible in all parts of the app.
        bootstrap — The main application view, called the root component, which hosts all other app views. Only the root NgModule should set this bootstrap property.
            import { NgModule }      from '@angular/core';
            import { BrowserModule } from '@angular/platform-browser';
            @NgModule({
            imports:      [ BrowserModule ],
            providers:    [ Logger ],
            declarations: [ AppComponent ],
            exports:      [ AppComponent ],
            bootstrap:    [ AppComponent ]
            })
            export class AppModule { }
    - In JavaScript each file is a module and all objects defined in the file belong to that module. The module declares some objects to be public by marking them with the export key word. Other JavaScript modules use import statements to access public objects from other modules.

### Components:
    - A component controls a patch of screen called a view.
    - The @Component decorator identifies the class immediately below it as a component class, and specifies its metadata
    - selector: A CSS selector that tells Angular to create and insert an instance of this component wherever it finds the corresponding tag in template HTML.
    - templateUrl: The module-relative address of this component's HTML template. Alternatively, you can provide the HTML template inline, as the value of the template property. This template defines the component's host view.
    - providers: An array of dependency injection providers for services that the component requires
    - After creating a component/directive by calling its constructor, Angular calls the lifecycle hook methods
    - ngOnChanges()
    - ngOnInit()
    - ngDoCheck()
    - ngAfterContentInit()
    - ngAfterContentChecked()
    - ngAfterViewInit()
    - ngAfterViewChecked()
    - ngOnDestroy()
    - Use ngOnInit() for two main reasons:
        To perform complex initializations shortly after construction.
        To set up the component after Angular sets the input properties.
    - ngOnChanges() : Angular calls its ngOnChanges() method whenever it detects changes to input properties of the component (or directive).
    - Use the DoCheck hook to detect and act upon changes that Angular doesn't catch on its own.
    - AfterViewInit() and AfterViewChecked() hooks that Angular calls after it creates a component's child views.

### Directives:
    - There are three kinds of directives in Angular:
        - Components—directives with a template.
        - Structural directives — change the DOM layout by adding and removing DOM elements.
        - Attribute directives — change the appearance or behavior of an element, component, or another directive.

### Pipes:
    - A pipe takes in data as input and transforms it to a desired output.
    - Pure Pipe : Angular executes a pure pipe only when it detects a pure change to the input value. (default)
    - Impure Pipe : Angular executes an impure pipe during every component change detection cycle. An impure pipe is called often, as often as every keystroke or mouse-move.
    - Async Impure Pipe : The AsyncPipe accepts a Promise or Observable as input and subscribes to the input automatically, eventually returning the emitted values

```javascript
import { Pipe, PipeTransform } from '@angular/core';
@Pipe({name: 'exponentialStrength'})
export class MyCustomPipe implements PipeTransform {
  transform(value: number, arg: string): number {
    // do something with number 
    return number;
  }
}
```

### Observables:
    - Observables are declarative—that is, you define a function for publishing values, but it is not executed until a consumer subscribes to it. The subscribed consumer then receives notifications until the function completes, or until they unsubscribe.##
    
### Template reference variables ( #var )
    - A template reference variable is often a reference to a DOM element within a template. It can also be a reference to an Angular component or directive or a web component.
    - Use the hash symbol (#) to declare a reference variable. The #phone declares a phone variable on an <input> element.
