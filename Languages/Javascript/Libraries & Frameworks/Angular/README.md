
- [General](#general)
- [Architecture](#architecture)
  - [Modules](#modules)
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
  - [Directives](#directives)
    - [ngFor](#ngfor)
    - [ngIf](#ngif)
    - [ngSwitch](#ngswitch)
    - [ngClass](#ngclass)
    - [ngStyle](#ngstyle)
    - [ngModel](#ngmodel)
- [Custom Events](#custom-events)


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

* **NgModules**
* Provides compilation context for components.
* Every angular app always has at least one **root module** that enables bootstrapping, and typically has many more feature modules.
* Can contain **components** and **services**.
* NgModules can import functionality from other NgModules.
* NgModules allows their own functionality to be exported and used by other NgModules.
* The components that belong to an NgModule **share a compilation context**.


* Metadate is the options object that defines the module behaviour.
* Some of these options like below:
  * **declarations**
    * Components, pipes and directives that belong to this module.
  * **exports**
    * Subset of declarations that visible and usable in the *component templates* of the other modules.
  * **imports**
    * Other modules that are needed by this module.
  * **providers**
    * An array of providers for services that the module requires.
  * **bootstrap**
    * The main application view (**root component**)
    * Only the root NgModule should set the bootstrap property

```ts
// @FILE: src/app/app.module.ts

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({
    imports: [ BrowserModule ],
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


* Metadate is the options object that defines the component behaviour.
* Some of these options like below:
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




## Directives
* Angular transforms the DOM according to the instructions given by directives.
* Angular defines a number of directives.
* You can define your own using the @Directive() decorator.
* In templates, directives typically appear within an element tag as attributes.
* There is two kind of directives:
  * **Structural directives** alter layout by adding, removing, and replacing elements in the DOM. 
  
    ```html
    <li *ngFor="let hero of heroes"></li>
    <app-hero-detail *ngIf="selectedHero"></app-hero-detail>
    ```

  * **Attribute directives**  alter the appearance or behavior of an existing element. 
  
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