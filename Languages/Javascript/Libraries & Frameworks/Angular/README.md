
- [General](#general)
- [Architecture](#architecture)
  - [Modules](#modules)
  - [Components](#components)
  - [Services](#services)
- [Dependency Injection (DI)](#dependency-injection-di)
  - [Injector](#injector)
  - [Provider](#provider)
- [Templating](#templating)
  - [Data Binding](#data-binding)
  - [Pipes](#pipes)
  - [Directives](#directives)
    - [ngFor](#ngfor)
    - [ngIf](#ngif)


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
    * **template**
      * inline HTML template of this component.
    * **providers**
      * An array of providers for services that the component requires.

```ts
// @FILE: src/app/hero-list.component.ts

@Component({
    selector: 'app-hero-list',
    templateUrl: './hero-list.component.html',
    providers:  [ HeroService ]
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

## Data Binding
* Binding can be 
  * interpolation ( mustache syntax {{ }} )
  * property binding
  * event binding
  * two-way data binding with **model**
* Angular processes all data bindings once for each JavaScript event cycle, from the root of the application component tree through all child components.

```html
<li>{{hero.name}}</li>
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
<li (click)="selectHero(hero)"></li>
<input [(ngModel)]="hero.name">
```

![](./ng-databinding.png)


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
<li *ngFor="let item of items; index as i;">
    <span> {{ i }}. {{ item.name }}</span>
</li>
```

* Some additioanl special keywords that can be used with *as* operator:
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