
- [General](#general)
- [Architecture](#architecture)
  - [Modules](#modules)
    - [Frequently Used Modules](#frequently-used-modules)
  - [Components](#components)
  - [Services](#services)
- [Dependency Injection (DI)](#dependency-injection-di)
  - [Injector](#injector)
  - [Provider](#provider)
- [Templating](#templating)
  - [Template References](#template-references)
  - [Data Binding](#data-binding)
    - [Attribute Binding](#attribute-binding)
    - [Class Binding](#class-binding)
    - [Style Binding](#style-binding)
    - [Event Binding](#event-binding)
    - [Two-way Binding](#two-way-binding)
  - [Pipes](#pipes)
  - [Safe Navigation Operator](#safe-navigation-operator)
  - [Non-Null Assertion Operator](#non-null-assertion-operator)
  - [Directives](#directives)
    - [ngFor](#ngfor)
    - [ngIf](#ngif)
    - [ngSwitch](#ngswitch)
    - [ngClass](#ngclass)
    - [ngStyle](#ngstyle)
    - [ngModel](#ngmodel)
- [Custom Events](#custom-events)
  - [EventEmitter](#eventemitter)
- [Input/Output](#inputoutput)
  - [Input](#input)
  - [Output](#output)
- [Lifecycle Hooks](#lifecycle-hooks)
  - [OnChanges](#onchanges)
  - [OnInit](#oninit)
  - [DoCheck](#docheck)
  - [AfterContentInit](#aftercontentinit)
  - [AfterContentChecked](#aftercontentchecked)
  - [AfterViewInit](#afterviewinit)
  - [AfterViewChecked](#afterviewchecked)
  - [OnDestroy](#ondestroy)
- [Styling](#styling)
  - [Component Styles Metadata](#component-styles-metadata)
  - [Component StyleUrls Metadata](#component-styleurls-metadata)
  - [Template Inline Styles](#template-inline-styles)
  - [Special Selectors](#special-selectors)
    - [:host](#host)
    - [:host-context](#host-context)
- [Observables](#observables)
  - [Basic Usage and Terms](#basic-usage-and-terms)
  - [Naming Conventions for Observables](#naming-conventions-for-observables)
  - [Defining Observers](#defining-observers)
  - [Creating Observables](#creating-observables)
  - [Subscribing](#subscribing)
  - [RxJS](#rxjs)
    - [RxJS Observable Creation Functions](#rxjs-observable-creation-functions)
    - [RxJS Operators](#rxjs-operators)
      - [Error Handling with catchError](#error-handling-with-catcherror)
      - [Retry on Error](#retry-on-error)
    - [RxJS Subjects (Multicast)](#rxjs-subjects-multicast)
    - [Subject](#subject)
    - [ReplaySubject](#replaysubject)
    - [BehaviorSubject](#behaviorsubject)
    - [BehaviorSubject](#behaviorsubject-1)


# General

* This document written for **Angular version 7.2.14**
* Written with [**Typescript**](../../../Typescript/README.md)



# Architecture

![](./ng-overview1.png)

## Modules
```ts
// Module decorator
@NgModule(metadata)
```

* **NgModule**
* Provides compilation context for components.
* Every angular app always has at least one **root module** that enables bootstrapping, and typically has many more feature modules.
* Can contain **components**, **services**, **directives** and **pipes**.
* NgModules can import functionality from other NgModules.
* NgModules allows their own functionality to be exported and used by other NgModules.
* The components that belong to an NgModule **share a compilation context**.
* Metadata is the options object that defines the module behaviour. Some of these options like below:
  * **declarations**
    * Components, pipes and directives that belong to this module.
    * You must declare every component in exactly one NgModule class.
    * You only need to declare them **once** in your app because you share them by importing the necessary **modules**.
  * **exports**
    * Makes some declarations of those components, directives, and pipes public so that other module's component templates can use them.
  * **imports**
    * Other **modules** that are needed by this module.
    * A component template **can reference** another component, directive, or pipe when the referenced item is declared in **this module** or another **imported module**
  * **providers**
    * An array of providers for services that the module requires.
    * Provides services that the other application components can use.
  * **bootstrap**
    * The main application view (**root component**)
    * Only the root NgModule should set the bootstrap property
    * Can be array
* NgModules brings **components**, **directives**, and **pipes** together into cohesive blocks of functionality, each focused on a feature area, application business domain, workflow, or common collection of utilities.
* Modules can be loaded when the application starts or lazy loaded asynchronously by the router.

### Frequently Used Modules

| NgModule | Import it from | Why you use it |
| --- | --- | --- |
| BrowserModule | @angular/platform-browser | When you want to run your app in a browser
| CommonModule | @angular/common | When you want to use NgIf, NgFor
| FormsModule | @angular/forms | When you want to build template driven forms (includes NgModel)
| ReactiveFormsModule | @angular/forms | When you want to build reactive forms
| RouterModule | @angular/router | When you want to use RouterLink, .forRoot(), and .forChild()
| HttpClientModule | @angular/common/http | When you want to talk to a server


```ts
// @FILE: src/app/app.module.ts

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({
    imports: [ BrowserModule ], // includes common things
    providers: [ Logger ],
    declarations: [ AppComponent ],
    exports: [ AppComponent ],
    bootstrap: [ AppComponent ]
})
export class AppModule { }
```


## Components
```ts
// Component decorator
@Component(metadata)
```

* Defines and controls **views**.
* Uses **services** which provide specific functionality not directly related to views.
* The class interacts with the view through *properties* and *methods*.
* Service providers can be **injected** into components as *dependencies*.
* Every angular app always has at least one **root module**.
* Each component
  * defines a class that contains application data and logic.
  * is associated with an HTML template that defines a view.
* Metadata is the options object that defines the component behaviour. Some of these options like below:
  * **selector**
    * HTML tag that the component is inserted into.
  * **templateUrl**
    * address of this component's HTML template.
  * **styleUrls**
    * address of this component's styles.
  * **template**
    * inline HTML template of this component.
  * **providers**
    * An array of providers for services that the component requires.

```ts
// @FILE: src/app/hero-list.component.ts

@Component({
    selector: 'app-hero-list',
    templateUrl: './hero-list.component.html',
    styleUrls: [ './hero-list.component.css'],
    providers: [ HeroService ]
})
export class HeroListComponent implements OnInit {
/* ... */
}
```

* See also:
  * [Templating](#templating)


## Services
```ts
// Service decorator
@Injectable()
```

* For data or logic that isn't associated with a specific view, and that you want to share across components.
* A service is typically a class with a narrow, well-defined purpose. 
* It should do something specific and do it well.
* A component can delegate certain tasks to services, such as fetching data from the server, validating user input, or logging directly to the console.
* By defining such processing tasks in an injectable service class, you make those tasks available to any component. 
* You can also make your app more adaptable by injecting different providers of the same kind of service.

```ts
// @FILE: src/app/logger.service.ts

export class Logger {
    log(msg: any) { console.log(msg); }
    error(msg: any) { console.error(msg); }
    warn(msg: any) { console.warn(msg); }
}
```

* Services can depend on other services.

```ts
// @FILE: src/app/hero.service.ts

export class HeroService {
    private heroes: Hero[] = [];

    constructor(
        private backend: BackendService,
        private logger: Logger
    ) {}

    getHeroes() {
        this.backend.getAll(Hero).then( (heroes: Hero[]) => {
            this.logger.log(`Fetched ${heroes.length} heroes.`);
            this.heroes.push(...heroes); // fill cache
        });
        return this.heroes;
    }
}
```

* See also:
  * [Dependency Injection (DI)](#dependency-injection-di)





# Dependency Injection (DI)
* It is used everywhere to provide new components with the services or other things they need.
* To define a class as a service in Angular, use the **@Injectable()** decorator to allow Angular to inject it into a component as a dependency.
* A dependency doesn't have to be a service—it could be a function, for example, or a value.
* When Angular creates a new instance of a component class, it determines which services or other dependencies that component needs by looking at the **constructor parameter types**.

## Injector
* The **injector** is the main mechanism.
* An injector creates dependencies, and maintains a container of dependency instances that it reuses if possible. 
* A **provider** is an object that tells an injector how to obtain or create a dependency.

## Provider
* You must register at least one provider of any service you are going to use.
* The provider can be part of the service's own metadata, making that service available everywhere or you can register providers with specific modules or components. 
* You can register providers:
  * in the service metadata (with @Injectable() decorator)
  * in the module metadata (with @NgModule() decorator)
  * in the component metadata (with @Component() decorator)






# Templating
* A template combines HTML with Angular markup that can modify HTML.
* Templates can use:
  * *data binding* to coordinate the app and DOM data
  * *pipes* to transform data before it is displayed
  * *directives* to apply app logic to what gets displayed

## Template References
* It is like a **:ref="referenceName"** directive at the Vue.JS (**$refs**)
* A template reference variable is often a reference to a DOM element within a template. 
* It can also be a reference to:
  * Angular component
  * directive
  * web component.
* The scope of a reference variable is the entire template.

```html
<!-- @FILE: src/app/app.component.html -->

<input #phone placeholder="phone number">
<!-- OR -->
<input ref-email placeholder="email address">

<button (click)="callPhone(phone.value)">Call</button>
```



## Data Binding

![](./ng-databinding.png)

* Binding can be 
  * interpolation
    ```html
    <div>{{ statement }}</div>
    ```
  * property binding
    ```html
    <div [target]="statement"> ... </div>
    <!-- OR -->
    <div bind-target="statement"> ... </div>
    ```
  * event binding
    ```html
    <div (target)="statement"> ... </div>
    <!-- OR -->
    <div on-target="statement"> ... </div>
    ```
  * two-way data binding with **model**
    ```html
    <div [(target)]="statement"> ... </div>
    <!-- OR -->
    <div bindon-target="statement"> ... </div>
    ```
* Angular processes all data bindings once for each JavaScript event cycle, from the root of the application component tree through all child components.
* Valid javascript statements can be used inside the interpolation syntax. (**template expressions**)
  * operators
  * function calls
  * ternary operator
  * etc.
* All data bound properties must be TypeScript **public** properties. Angular never binds to a TypeScript **private** property.

```html
<li>{{hero.name}}</li>
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
<li (click)="selectHero(hero)"></li>
<input [(ngModel)]="hero.name">
```

### Attribute Binding
* Some of html attributes are not considered as html properties like:
  * colspan
  * aria
  * svg
* These kind of attributes can be bound to the element with using "**attr**" namespace.

```html
<button [attr.aria-label]="actionName">{{actionName}} with Aria</button>

<table border=1>
  <tr><td [attr.colspan]="1 + 1">One-Two</td></tr>
</table>
```

### Class Binding
* Overrides standart class property.
```html
<div class="bad curly special" [class]="badCurly"> ... </div>
```

* Specific class can be bound with "**class**" name space. Its statement must be truthy/falsy value.
```html
<div [class.special]="!isNormal"> ... </div>
```

* See Also: [ngClass](#ngclass)


### Style Binding
* You can set inline styles with a style binding
* All style bindings must be bound with namespace dot(.) notation.
* Style properties can be written in:
  * camelCase (ex: fontSize)
  * dash-case (ex: font-size)

```html
<button [style.color]="isSpecial ? 'red': 'green'">Red</button>
```

* See Also: [ngStyle](#ngstyle)


### Event Binding
```html
<a (click)="linkClicked()">Click Me!</a>
```

* Event binding statement can include data values such as an event object, string, or number with name special **$event** variable.
* The target event determines the shape of the $event object. 
* If the target event is a native DOM element event, then $event is a DOM event object.
  
```html
<a (click)="linkClicked($event)">Click Me!</a>

<input [value]="currentItem.name" (input)="currentItem.name=$event.target.value" />
```
* See also: [Custom Events](#custom-events)

### Two-way Binding
* Angular offers a special two-way data binding syntax for this purpose, **[(prop)]**
* The **[(prop)]** syntax combines the brackets of property binding, **[prop]**, with the parentheses of event binding, **(prop)**.
* The **[(prop)]** syntax means
  * the element has a settable property called **prop**
  * a corresponding event named **propChange**

```ts
// @FILE: src/app/size/sizer.component.ts

import { Component, EventEmitter, Input, Output } from '@angular/core';

@Component({
    selector: 'app-sizer',
    template: `
    <div>
        <button (click)="dec()" title="smaller">-</button>
        <button (click)="inc()" title="bigger">+</button>
        <label [style.font-size.px]="size">FontSize: {{size}}px</label>
    </div>`
})
export class SizerComponent {
    @Input() size: number | string;
    @Output() sizeChange = new EventEmitter<number>();
    
    dec() { this.resize(-1); }
    inc() { this.resize(+1); }
    
    resize(delta: number) {
        this.size = Math.min(40, Math.max(8, +this.size + delta));
        this.sizeChange.emit(this.size);
    }
}
```

```html
<!-- @FILE: src/app/app.component.html -->

<app-sizer [(size)]="fontSizePx"></app-sizer>
<div [style.font-size.px]="fontSizePx">Resizable Text</div>
```

* See Also: [ngModel](#ngmodel)




## Pipes
* Angular pipes let you declare display-value transformations in your template HTML
* A class with the decorator below defines a function that transforms input values to output values.

```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ 
    name: 'filesize' 
})
export class FileSizePipe implements PipeTransform {
    transform(size: number, extension: string = 'MB', decimal: number = 2): string {
        return (size / (1024 * 1024)).toFixed(decimal) + extension;
    }
}
```

* To specify a value transformation in an HTML template, use the pipe operator (|).

```html
<span>{{ file.size | filesize:'MB' }}</span>
<span>{{ file.size | filesize:'megabyte':3 }}</span>
```

* Angular has various [pipes](https://angular.io/api?type=pipe).
* You can chain pipes, sending the output of one pipe function to be transformed by another pipe function. 


## Safe Navigation Operator
* Safe navigation operator (?.) is a fluent and convenient way to guard against null and undefined values in property paths. 

```html
<p> Value is: {{ a?.b?.c?.d }} </p>
```

## Non-Null Assertion Operator
* The non-null assertion operator does not guard against null or undefined.
* It tells the TypeScript type checker to suspend strict null checks for a specific property expression.

```html
<div *ngIf="a">
    <p> Value is: {{ a!.b }} </p>
</div>
```


## Directives
* Angular transforms the DOM according to the instructions given by directives.
* Angular defines a number of directives.
* You can define your own using the @Directive() decorator.
* In templates, directives typically appear within an element tag as attributes.
* There is two kind of directives:
  * **Structural directives** alter layout by adding, removing, and replacing elements in the DOM. 
  * Used with asteriks. (*)
  
    ```html
    <li *ngFor="let hero of heroes"></li>
    <app-hero-detail *ngIf="selectedHero"></app-hero-detail>
    ```

  * **Attribute directives**  alter the appearance or behavior of an existing element. 
  * Used inside brackets.
  
    ```html
    <input [(ngModel)]="hero.name">
    ```


### ngFor
* It renders a template for each item in a collection.

```html
<li *ngFor="let item of items;">
    <span> {{ item.name }}</span>
</li>
```

* Accessing iteration number with special **index** keyword.

```html
<li *ngFor="let item of items; let i = index;">
    <span> {{ i }}. {{ item.name }}</span>
</li>
```

* Some additioanl special keywords that can be used with *assignment* operator:
  * **even**: boolean
  * **odd**: boolean
  * **first**: boolean
  * **last**: boolean


### ngIf
* Conditional rendering.

```html
<div *ngIf="items.length">
    <li *ngFor="let item of items;">
        <span> {{ item.name }}</span>
    </li>
</div>
```


### ngSwitch
* NgSwitch is like the JavaScript switch statement. 
* It can display one element from among several possible elements, based on a switch condition.
* Works with these:
  * **ngSwitchCase**
  * **ngSwitchDefault**

```html
<div [ngSwitch]="emotion">
  <p *ngSwitchCase="'happy'">I am happy :) </p>
  <p *ngSwitchCase="'sad'">I am sad :( </p>
  <p *ngSwitchCase="'confused'">I am confused :| </p>
  <p *ngSwitchDefault>I fell nothing </p>
</div>
```



### ngClass
* You can bind to the ngClass to add or remove several classes simultaneously.
* Each key of the object is a CSS class name; its value is true if the class should be added, false if it should be removed.

```ts
// @FILE: src/app/app.component.ts
...
currentClasses: {};
setCurrentClasses () {
    this.currentClasses = {
        saveable: this.canSave,
        modified: !this.isUnchanged,
        special: this.isSpecial
    };
}
...
```

```html
<!-- @FILE: src/app/app.component.html -->
<div [ngClass]="currentClasses"></div>
```


### ngStyle
* You can set inline styles dynamically, based on the state of the component.
* Each key of the object is a style name; its value is whatever is appropriate for that style.

```ts
// @FILE: src/app/app.component.ts
...
currentStyles: {};
setCurrentStyles () {
    this.currentStyles = {
        'font-style': this.canSave ? 'italic' : 'normal',
        'font-weight': !this.isUnchanged ? 'bold' : 'normal',
        'font-size': this.isSpecial ? '24px' : '12px'
    };
}
...
```

```html
<!-- @FILE: src/app/app.component.html -->
<div [ngStyle]="currentStyles"></div>
```


### ngModel
* Easy way of the two-way data binding.
* FormsModule is required to use ngModel.
* You must **import** the FormsModule and add it to the NgModule's **imports list**.
* NgModel directive only works for an element supported by a **ControlValueAccessor** that adapts an element to this protocol.

```ts
// @FILE: src/app/app.module.ts

import { FormsModule } from '@angular/forms';

@NgModule({
    imports: [ FormsModule ],
})
export class AppModule {}
```

```html
<!-- @FILE: src/app/app.module.html -->
<input [(ngModel)]="inputDataHolder">
```






# Custom Events
* Can be done with EventEmitter.

```html
<!-- @FILE: src/app/subcomponent/subcomponent.component.html -->
<button (click)="delete()">Delete</button>
```

```ts
// @FILE: src/app/subcomponent/subcomponent.component.ts

...

@Output() deleteRequest = new EventEmitter<Item>();

delete() {
  this.deleteRequest.emit(this.item.name);
}

...
```

```html
<!-- @FILE: src/app/app.component.html -->
<subcomponent (deleteRequest)="deleteRequested($event)"></subcomponent>
```

## EventEmitter
// TODO: write down





# Input/Output
* You can always bind to a public property of a component in its own template. 
* It doesn't have to be an Input or Output property.
* The Angular compiler won't bind to properties of a different component unless they are Input or Output properties.

## Input
```ts
@Input(alias) propertyName: type;
```

* An Input property is a settable property annotated with an **@Input** decorator.
* It is used to descibe component properties.

## Output
```ts
@Output(alias) propertyName = new EventEmitter<type>();
```

* An Output property is an observable property annotated with an **@Output** decorator. 
* The property almost always returns an Angular EventEmitter.
* It is used to descibe component events.


```ts
// FILE: src/app/some-component/some-component.component.ts
@Input() item: any;
@Output('accessed') itemAccessed = new EventEmitter<any>();
```

```ts
// FILE: src/app/some-component/some-component.component.ts
@Component({
    selector: 'some-component',
    inputs: ['item'],
    outputs: ['accessed:itemAccessed'],
})
export class SomeComponent {

}
```


# Lifecycle Hooks
* Directive and component instances have a lifecycle as Angular creates, updates, and destroys them.
* Lifecycles are defined with lifecycle hook **interfaces**.
* Components and directives must **implement** these interfaces to use hooks.
* Other Angular sub-systems may have their own lifecycle hooks apart from these component hooks.
* Each interface has a single hook method whose name is the interface name prefixed with ng.
  * Ex.: the **OnInit** interface has a hook method named **ngOnInit()**

```ts
export class Example implements OnInit {
    constructor () { }

    ngOnInit () { 
        console.log('onInit called.'); 
    }
}
```

## OnChanges
* A lifecycle hook that is called when any data-bound property of a directive changes.
* Receives a **SimpleChanges** object of current and previous property values. 
* Called before *OnInit* and data-bound input properties change.

## OnInit
* A lifecycle hook that is called after Angular has initialized all data-bound properties of a directive.
* Called once, after the first OnChanges.

## DoCheck
* A lifecycle hook that invokes a custom change-detection function for a directive, in addition to the check performed by the default change-detector.
* Called during every change detection run, immediately after OnChanges and OnInit.

## AfterContentInit
* A lifecycle hook that is called after Angular has fully initialized all content of a directive.
* Called once after the first DoCheck.

## AfterContentChecked
* A lifecycle hook that is called after the default change detector has completed checking all content of a directive.
* Called after the AfterContentInit and every subsequent DoCheck.

## AfterViewInit
* A lifecycle hook that is called after Angular has fully initialized a component's view.
* Called once after the first AfterContentChecked.

## AfterViewChecked
* A lifecycle hook that is called after the default change detector has completed checking a component's view for changes.
* Called after the AfterViewInit and every subsequent AfterContentChecked.

## OnDestroy
* A lifecycle hook that is called when a directive, pipe, or service is destroyed. 
* Use for any custom cleanup that needs to occur when the instance is destroyed.
* Unsubscribe Observables and detach event handlers to avoid memory leaks. 
* Called just before Angular destroys the directive/component.



# Styling

## Component Styles Metadata
* Used with **@Component** decorator's **style** metadata array.
* The styles specified in this method apply only within the template of that component.
* They are **not inherited** by any components nested within the template nor by any content projected into the component.
* They must be written in plain CSS.

```ts
/* @FILE: src/app/custom-element.component.ts */

@Component({
    selector: 'app-root',
    template: `
        <h1>Header Text</h1>
        <custom-element></custom-element>
    `,
    styles: [ 'h1 { font-weight: normal; }' ]
})
export class CustomElementComponent {
}
```

## Component StyleUrls Metadata
* Used with **@Component** decorator's **style** metadata array.
* The styles specified in this method apply only within the template of that component.
* They are **not inherited** by any components nested within the template nor by any content projected into the component.
* Non-CSS style files are valid also like:
  * sass/scss
  * less
  * stylus

```ts
/* @FILE: src/app/custom-element.component.ts */

@Component({
    selector: 'app-root',
    template: `
        <h1>Header Text</h1>
        <custom-element></custom-element>
    `,
    styleUrls: [ './custom-element.component.css' ]
})
export class CustomElementComponent {
}
```

## Template Inline Styles
* You can embed CSS styles directly into the HTML template by putting them inside **style** tags.

```ts
/* @FILE: src/app/custom-element.component.ts */

@Component({
    selector: 'app-root',
    template: `
        <style>
            h1 { font-weight: normal; }
        </style>
        <h1>Header Text</h1>
        <custom-element></custom-element>
    `,
})
export class CustomElementComponent {
}
```

## Special Selectors
* Component styles have a few special selectors

### :host
* Use the :host pseudo-class selector to target styles in the element that hosts the component (as opposed to targeting elements inside the component's template).
* It is the only way to target the host element.
* It has two forms:
  * pseudo-class
  * function
* Use the function form to apply host styles conditionally by including another selector inside parentheses.

```css
/* @FILE: src/app/custom-element.component.css */

:host(.active) {
    border-width: 3px;
}
```

### :host-context
* The :host-context() selector looks for a CSS class in any ancestor of the component host element, up to the document root. 
* The following example applies a background-color style to all <h2> elements inside the component, only if some ancestor element has the CSS class theme-light.

```ts
/* @FILE: src/app/custom-element.component.css */

:host-context(.theme-light) h2 {
    background-color: #eef;
}
```




# Observables
* Provides support for passing messages between publishers and subscribers in your application.
* An observable can deliver multiple values of any type like:
  * literals
  * messages
  * events
  * etc...
* Your application code only needs to worry about subscribing to consume values, and when done, unsubscribing.
* See also
  * [RxJS](https://www.learnrxjs.io/): It has a tons of predefined observables.

## Basic Usage and Terms

* As a **publisher**, you create an **Observable** instance that defines a *subscriber function*.
  * This is the function that is executed when a consumer calls the **subscribe()** method.
  * The *subscriber function* defines how to obtain or generate values or messages to be published.
* To execute the observable:
  * you have created and begin receiving notifications
  * you call its **subscribe()** method, passing an **observer**
  * This is a JavaScript object that defines the handlers for the notifications you receive. 
  * The **subscribe()** call returns a **Subscription** object that has an **unsubscribe()** method, which you call to stop receiving notifications.


## Naming Conventions for Observables
* You will often see observables named with a trailing “$” sign.
* This can be useful when scanning through code and looking for observable values. 
* Also, if you want a property to store the most recent value from an observable, it can be convenient to simply use the same name with or without the “$”.

```ts
import { Component } from '@angular/core';
import { Observable } from 'rxjs';
    
@Component({
    // ...
})
export class StopwatchComponent {
    stopwatchValue: number;
    stopwatchValue$: Observable<number>;
    
    start() {
        this.stopwatchValue$.subscribe((num) =>
            this.stopwatchValue = num;
        );
    }
}
```


## Defining Observers
* A handler for receiving observable notifications implements the Observer interface.
* **Observer interface** type is used
  * when creating Observable instance at its callback argument type
  * at **Observable.subscribe()** method's argument type

```ts
let observer: Observer = {
    next: function(val) {},
    error: function(msg) {}, // optional
    complete?: function() {} // optional
}
```


## Creating Observables
* Use the Observable constructor to create an observable stream of any type. 
* The constructor takes as its argument the subscriber function to run when the observable’s subscribe() method executes. 
* A subscriber function receives an [Observer](#defining-observers) object, and can **publish** values to the observer's **next()** method.

```ts
const _observable$ = new Observable((observer: Observer) => {
    // synchronously deliver 1, 2, and 3, then complete
    observer.next(1);
    observer.next(2);
    observer.next(3);
    observer.complete();
    
    return {
        unsubscribe() {
            // unsubscribe function doesn't need to do anything in this
            // because values are delivered synchronously
        }
    };
});
```

* Another example:

```ts
function eventStream (target, eventName) {
    return new Observable((observer: Observer) => {
        const handler = (e) => {
            observer.next(e);
        };

        target.addEventListener(eventName, handler);

        return {
            unsubscribe() {
                target.removeEventListener(eventName, handler);
            }
        };
    });
}

// Some Other File:
const btnElement = document.getElementById('btn') as HTMLInputElement;
const buttonSubscription = eventStream(btnElement, 'focus').subscribe((ev) => {
    btnElement.blur();
});
```


## Subscribing
* An Observable instance begins publishing values only when someone subscribes to it. 
* You subscribe by calling the 
  * **Observable.subscribe(observerObj)**
  * **Observable.subscribe(nectHandler, errorHandler, completeHandler)**

```ts
const _observable$ = new Observable((observer: Observer) => {
    // ...
    // do something and then call 
    // * observer.next() or
    // * observer.error() or
    // * observer.complete()
});

// subscribe(observer: Observer): any;
_observable$.subscribe({
    next (val) {},
    error (msg) {}, // optional
    complete () {}, // optional
});

// OR
_observable$.subscribe(
    (val) => {
        // next handler
    },
    (msg) => {
        // error handler / optional
    },
    () => {
        // complete handler / optional
    }
});
```


## RxJS
* RxJS (Reactive Extensions for JavaScript) is a library for **reactive programming** using **observables** that makes it easier to compose asynchronous or callback-based code
* RxJS provides an implementation of the Observable type
* The library also provides utility functions for creating and working with observables. 
* These utility functions can be used for:
  * Converting existing code for async operations into observables
  * Iterating through the values in a stream
  * Mapping values to different types
  * Filtering streams
  * Composing multiple streams
* Must see also:
  * [https://rxjs-dev.firebaseapp.com/api](https://rxjs-dev.firebaseapp.com/api)
  * [https://www.learnrxjs.io/](https://www.learnrxjs.io/)

### RxJS Observable Creation Functions
* RxJS offers a number of functions that can be used to create new observables
* These functions can simplify the process of creating observables from things such as events, timers, promises, and so on.

```ts
// Create an observable from a promise

import { from } from 'rxjs';

// Create an Observable out of a promise
const data$ = from(fetch('/api/endpoint'));

data$.subscribe({
    next(response) { console.log(response); },
    error(err) { console.error('Error: ' + err); },
    complete() { console.log('Completed'); }
});
```

```ts
// Create an observable from an event
import { fromEvent } from 'rxjs';
    
const el = document.getElementById('my-element');
const mouseMoves$ = fromEvent(el, 'mousemove');
const subscription = mouseMoves$.subscribe((evt: MouseEvent) => {
    // Log coords of mouse movements
    console.log(`Coords: ${evt.clientX} X ${evt.clientY}`);
    
    if (evt.clientX < 40 && evt.clientY < 40) {
        subscription.unsubscribe();
    }
});
```

* Some of these functions like below:
  * [ajax](https://www.learnrxjs.io/operators/creation/ajax.html)
    * Create an observable for an Ajax request with either a request object with url, headers, etc or a string for a URL.
  * [create](https://www.learnrxjs.io/operators/creation/create.html)
    * Create an observable with given subscription function.
  * [from](https://www.learnrxjs.io/operators/creation/from.html)
    * Turn an array, promise, or iterable into an observable.
  * [fromEvent](https://www.learnrxjs.io/operators/creation/fromevent.html)
    * Turn event into observable sequence.
  * [interval](https://www.learnrxjs.io/operators/creation/interval.html)
    * Emit numbers in sequence based on provided timeframe.
  * [of](https://www.learnrxjs.io/operators/creation/of.html)
    * Emit variable amount of values in a sequence.
  * [range](https://www.learnrxjs.io/operators/creation/range.html)
    * Emit numbers in provided range in sequence.
  * [throw](https://www.learnrxjs.io/operators/creation/throw.html)
    * Emit error on subscription.
  * [timer](https://www.learnrxjs.io/operators/creation/timer.html)
    * After given duration, emit numbers in sequence every specified duration.
* See also: [https://www.learnrxjs.io/operators/creation/](https://www.learnrxjs.io/operators/creation/)


### RxJS Operators
* Operators are functions that build on the observables to enable sophisticated manipulation of collections.
* You can use pipes to link operators together. 
* Pipes let you combine multiple functions into a single function.
* Pipe has two version:
  * Generic function
    * takes as its arguments the functions you want to combine, and returns a new function 
    * when executed, runs the composed functions in sequence
        ```ts
        import { filter, map } from 'rxjs/operators';
        const nums$ = of(1, 2, 3, 4, 5);

        // Creates a function that accepts an Observable.
        const squareOddVals = pipe(
            filter((n: number) => n % 2 !== 0),
            map(n => n * n)
        );
        
        // Create an Observable that will run the filter and map functions
        const squareOdd$ = squareOddVals(nums$);
        squareOdd$.subscribe(x => console.log(x));
        ```
  * Observable instance function
    ```ts
    import { filter, map } from 'rxjs/operators';

    const squareOdd$ = of(1, 2, 3, 4, 5).pipe(
        filter(n => n % 2 !== 0),
        map(n => n * n)
    );

    // Subscribe to get values
    squareOdd$.subscribe(x => console.log(x));
    ```

* See also: [https://www.learnrxjs.io/operators/](https://www.learnrxjs.io/operators/)



#### Error Handling with catchError
* In addition to the error() handler that you provide on subscription, RxJS provides the **catchError** operator that lets you handle known errors in the observable recipe.
* If you catch error and supply a default value, your stream continues to process values rather than erroring out.

```ts
import { ajax } from 'rxjs/ajax';
import { map, catchError } from 'rxjs/operators';
const apiData$ = ajax('/api/data').pipe(
    map((res) => {
        if (!res.response) throw new Error('Value expected!');
        return res.response;
    }),
    catchError((err) => of([]))
);

apiData$.subscribe({
    next(x) { console.log('data: ', x); },
    error(err) { console.log('errors already caught... will not run'); }
});
```

#### Retry on Error
* Where the catchError operator provides a simple path of recovery, the retry operator lets you retry a failed request.
* Use the retry operator before the catchError operator. 
* It resubscribes to the original source observable, which can then re-run the full sequence of actions that resulted in the error. 


```ts
import { ajax } from 'rxjs/ajax';
import { map, catchError, retry } from 'rxjs/operators';
const apiData$ = ajax('/api/data').pipe(
    retry(3),
    map((res) => {
        if (!res.response) throw new Error('Value expected!');
        return res.response;
    }),
    catchError((err) => of([]))
);

apiData$.subscribe({
    next(x) { console.log('data: ', x); },
    error(err) { console.log('errors already caught... will not run'); }
});
```

### RxJS Subjects (Multicast)
* A Subject is a special type of **Observable** which shares a single execution path among **observers**. (multicast)
* Typical observables are 1 to 1.
* There are 4 variants of subjects:
  * Subject
    * No intial value or replay behavior.
  * ReplaySubject
    * Emits specified number of last emitted values (a replay) to new subscribers.
  * AsyncSubject
    * Emits latest value to observers upon completion.
  * BehaviorSubject
    * Requires an initial value and emits its current value (last emitted item) to new subscribers.


### Subject
* A special type of Observable which shares a single execution path among observers.
* Basic multicast. (No history, no initial)

```ts
import { Subject } from 'rxjs';

const sub = new Subject();

sub.next(1);
sub.subscribe(console.log);

sub.next(2); // OUTPUT => 2
sub.subscribe(console.log);

sub.next(3); // OUTPUT => 3,3 (logged from both subscribers)
```


### ReplaySubject
* It is an extended version of the standar Subject.
* It buffers a set number of values and will emit those values immediately to any new subscribers.

```ts
import { ReplaySubject } from 'rxjs';
const sub = new ReplaySubject(3);

sub.next(1);
sub.next(2);
sub.subscribe(console.log); 
// OUTPUT => 1,2

sub.next(3); // OUTPUT => 3
sub.next(4); // OUTPUT => 4
sub.subscribe(console.log); 
// OUTPUT => 2,3,4 (log of last 3 values from new subscriber)

sub.next(5); // OUTPUT => 5,5 (log from both subscribers)
```


### BehaviorSubject
* A variant of Subject that requires an initial value and emits its current value whenever it is subscribed to.
* It emits:
  * **initial value** to new subscribers if it has no history yet (never called next)
  * Else **last value** to new subscribers

```ts
import { BehaviorSubject } from 'rxjs';
const subject = new BehaviorSubject(123);

//two new subscribers will get initial value => output: 123, 123
subject.subscribe(console.log);
subject.subscribe(console.log);

//two subscribers will get new value => output: 456, 456
subject.next(456);

//new subscriber will get latest value (456) => output: 456
subject.subscribe(console.log);

//all three subscribers will get new value => output: 789, 789, 789
subject.next(789);

```


### BehaviorSubject
* It only emits the **last value** to all subscriber when **completed**.

```ts
import { AsyncSubject } from 'rxjs';
const sub = new AsyncSubject();

sub.subscribe(console.log);
sub.next(123); //nothing logged

sub.subscribe(console.log);
sub.next(456); //nothing logged

sub.complete(); //456, 456 logged by both subscribers
```