# Angular Style Guide

## Table of Contents
1. Starting a new project
2. Additional Tools.
3. Files Structure and Name Conventions
4. Creating new blocks - Angular CLI
5. Public API
6. HTML Templates
8. Semantic HTML
7. Styles
8. RxJS
9. Immutable
10. Types and Interfaces
11. Performance
12. Testing
13. PWA



## Starting a new project 
Make sure you are using latest AngularCLI.

Run `ng new` in your terminal.

Make sure you choose SCSS as a style preprocessor.

Recomended schematics for Angular Component:

```json
"@schematics/angular:component": {
  "style": "scss",
   "changeDetection": "OnPush"
 }
```


Angular set `app` as a default  prefix for Components and Directvies.

If you need to change it:

Ex: for `ui`
```json"
prefix": "ui",
```
 
 
 
## Additional Tools

### 1. Husky
Husky can prevent bad git commit, git push and more 🐶 woof!

Why?
1. To add some additional steps / tasks to do before/after commit/push. (ex. run tests)
2. To avoid bad commits. You can connect any Commit linter to check commit messages. 
    
    ex:
    ```json
      "husky": {
        "hooks": {
          "commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
          "pre-push": "npm run test-headless"
        }
      }
    ```

### 3. Conventional Commits

The Conventional Commits specification is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of. 

This convention dovetails with SemVer, by describing the features, fixes, and breaking changes made in commit messages. 

The commit message should be structured as follows:
```bash
    <type>[optional scope]: <description>
    [optional body]
    [optional footer(s)]
```
   
ex:
```bash
   feat: allow provided config object to extend other configs
   
   BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```
   
   Why?
   
   Provides a readable way of creating commit massages. No more "added some stuff" commits.
   
   **The same approach is also used by Angular Core Team.**

### 4. Linter 
It could be either TSLint (already included) or ESLint.
    
WARNING 
    
TSLint has been officially Deprecated and Migrating TSLint to ESLint is planned for Angular 10.
    
Recommended rules packages:

* Angular TSLint Rules https://github.com/fulls1z3/angular-tslint-rules
* TSConfig Prettier https://github.com/prettier/tslint-config-prettier

### 5. Prettier

Prettier is an opinionated code formatter. It enforces a consistent style by parsing your code and re-printing it with its own rules that take the maximum line length into account, wrapping code when necessary.
   
ex: `.prettierrc`

```json
   {
     "tabWidth": 2,
     "useTabs": false,
     "singleQuote": true,
     "trailingComma": "all",
     "bracketSpacing": true
   }
```

## Files Structure and Name Conventions [2](#files-structure-and-name-conventions)

### Structure

Files Structure should be divided, clear and understandable.

- <a name="files-structure-and-name-conventions--components"></a><a name="1.1"></a>[1.1](#files-structure-and-name-conventions--components) **Components**:
  It's a simple, single block of reusable (or not) code that represents some UI and logic. The best way is to keep our components "dummy" - not connected to the API, services, store, etc. All data should be served by parents (containers)

```bash
  |- Components
  |-- [component-name]
  |--- [component-name].component.ts
  |--- [component-name].component.html
  |--- [component-name].component.spec.ts
  |--- [component-name].component.scss
  |--- [component-name].module.ts
  ... or //
  |-- [group-name]
  |---- [component-name].component.ts
  |---- [component-name].component.html
  |---- [component-name].component.spec.ts
  |---- [component-name].component.scss
  |--- [group-name].module.ts
  |-- index.ts (public api export)
  |
```

- <a name="files-structure-and-name-conventions--containers"></a><a name="1.2"></a>[1.2](#files-structure-and-name-conventions--containers) **Containers**:
  Containers are wrappers for components. They provide all API connections, pass data, etc.

  Examples:

- forms
- gallery
- articles + images

```bash
  ...
  |- Containers
  |-- B-name
  |--- b-name.component.ts
  |--- b-name.component.html
  |--- b-name.component.spec.ts
  |--- b-name.component.scss
  |--- b-name.module.ts
  |-- B-name
  |--- b-name.component.ts
  |--- b-name.component.html
  |--- b-name.component.spec.ts
  |--- b-name.component.scss
  |--- b-name.module.ts
  |-- index.ts (public api export)
  |
  ...
```

- <a name="files-structure-and-name-conventions--views"></a><a name="1.3"></a>[1.3](#files-structure-and-name-conventions--views) **Views** 👀:

  Views are representation of each page visible for user (connected with routes and/or lazy loading modules). Views are wrapper for containers/components and provide most important APIs connections.

```bash
  |- views
  |-- [page-name]-view
  |--- [page-name]-routing.module.ts
  |--- [page-name].component.ts
  |--- [page-name].component.html
  |--- [page-name].component.spec.ts
  |--- [page-name].component.scss
  |--- [page-name].module.ts
  |-- [page-name]-view
  |--- [page-name]-routing.module.ts
  |--- [page-name].component.ts
  |--- [page-name].component.html
  |--- [page-name].component.spec.ts
  |--- [page-name].component.scss
  |--- [page-name].module.ts
  |-- index.ts (public api export)
```

- <a name="files-structure-and-name-conventions--modules"></a><a name="1.4"></a>[1.4](#files-structure-and-name-conventions--modules) **Modules**:

  Modules are boxes 📦 for some piece of code. It allows to share component declarations, pipes, modules between modules in the application.

```
  import { NgModule } from '@angular/core';
  import { CommonModule } from '@angular/common';

  // Modules
  import { ButtonsModule, FormsModule } from 'app/components';

  // Components
  import { ComponentA } from './component-a.component';
  import { ComponentB } from './component-b.component';

  const EXPORTS = [ComponentA, ComponentB];

  const UI_MODULES [ButtonsModule, FormsModule];

  @NgModule({
    imports: [
      CommonModule,
      ...UI_MODULES,
    ],
    declarations: [...EXPORTS],
    exports: [...EXPORTS]
  })
  export class CModule { }
```

> IMPORTANT
> **Routing modules should be loaded as LazyModules!**

- <a name="files-structure-and-name-conventions--services"></a><a name="1.5"></a>[1.5](#files-structure-and-name-conventions--services) **Services**:
  Services are usually used to consume some API, deliver some data or share data between components (not recommended - use Store instead)

```bash
  |- Services
  |-- [service-name]
  |--- [service-name].service.ts
  |--- [service-name].service.spec.ts
  ...
  |-- index.ts (public api export)
```

- <a name="files-structure-and-name-conventions--pipes"></a><a name="1.6"></a>[1.6](#files-structure-and-name-conventions--pipes) **Pipes**:
  Filters, sanitizers for our code.

```bash
  |- Pipes
  |-- [pipe-name]
  |--- [pipe-name].pipe.ts
  |--- [pipe-name].pipe.spec.ts
  ...
  |-- pipes.module.ts
  |-- pipes.module.spec.ts
  |-- index.ts (public api export)
```

- <a name="files-structure-and-name-conventions--interfaces"></a><a name="1.7"></a>[1.7](#files-structure-and-name-conventions--interfaces) **Interfaces**:
  With interfaces we define types of our data.

```bash
  |- Interfaces
  |-- [namespace-name]
  |--- [interface-name-one].interface.ts
  |--- [interface-name-two].interface.ts
  ...
  |-- index.ts (public api export)
```

**Export**: All files should be exported by `index.ts`.


#### OPTIONAL

**Access**:`tsconfig.json` should have applied aliases for paths.

```javascript
      "paths: {
        "app/components": ["src/components/index.ts"],
        "app/services": ["src/services/index.ts"],
        ...
      }
```

#### Usage:

```angular2
import { ServiceA } from "app/services";
```


**[⬆ back to top](#files-structure-and-name-conventions)**

## Creating new blocks - Angular CLI <a name="creating-new-blocks-angular-cli"></a><a name="2.0"></a>[1.1](#creating-new-blocks-angular-cli)

Angular CLI is a powerful tool. It does not only accelerates our work but also keep all files in the same "shape".

### To generate component/group in our file structure

ex: Shared buttons:
```bash
ng g m shared/components/buttons/primary-button
ng g c shared/components/buttons/primary-button
```

Recommended to use 1 Module for 1 Component.

### To generate services in our file structure

> Unfortunately Angular CLI doesn't support creating folders for services. You need to do it manually

```bash
ng g s services/[service-name-folder]/[service-name]
```

### ... and so one. Remember to use Angular CLI while you creating some new block!

**[⬆ back to top](#creating-new-blocks-angular-cli)**

## Public API <a name="public-api"></a><a name="3.0"></a>[3.0](#public-api)

> Why?
> To keep our import paths simple and clean.

Before:

```bash
|-root
|-- folder-a
|--- services
|---- user-service
|----- a-service.ts
|----- b-service.ts
...
|--- file1.ts
|--- file2.ts
```

```javascript
// I am in the b-service.ts NOW

import {module} from '../../helpers/file1.ts';
...

```

with the **public_api**:

```bash
|-root
|-- folder-a
|--- services
|---- user-service
|----- a-service.ts
|----- b-service.ts
...
|-- helpers
|--- file1.ts
|--- file2.ts
|--- index.ts //-- new file
```

in `index.ts`

```javascript
export * from "./file1.ts";
export * from "./file2.ts";
```

```javascript
// I am in the b-service.ts NOW

import {module} from 'app/helpers';
...

```

## HTML Templates <a name="html-templates"></a><a name="4.0"></a>[4.0](#html-templates)

There are 2 ways to write html code in Angular application.
There's no real difference between having an embedded template and an external template.

"...**For the developer, however, there are a number of differences that you have to consider.**

You get better code completion and inline support in your editor/IDE in most cases when the HTML is in a separate .html file. (IntelliJ IDEA, at least, supports HTML for inline templates and strings)
There is a convenience factor to having code and the associated HTML in the same file. It's easier to see how the two relate to each other.
These two things will be of equal value for many people, so you'd just pick your favorite and move ahead with life if this were all there was to it.
But that leads us to the reasons you should keep your templates in your components, in my opinion:
It is difficult to make use of relative file paths for external templates as it currently stands in Angular 2.
Using non-relative paths for external templates makes your components far less portable, since you need to manage all of the /where/is/my/template type references from the root that change depending on how deep your component is.
That's why I would suggest that you keep your templates inside your components where they are easily found. Also, if you find that your inline template is getting large and unwieldy, then it is probably a sign that you should be breaking your component down into several smaller components, anyway..."

> Source: StackOverflow

**My own opinion is to keep Component Template as a inline html. This way we force our self to keep component's code small.** - @Frank

> If you use VSCode to produce code I suggest to install this extension:
> https://marketplace.visualstudio.com/itemstemName=natewallace.angular2-inline
> that helps you to format HTML code in your .ts files

## Styles <a name="styles"></a><a name="5.0"></a>[5.0](#styles)

These points should be covered:

### Each component has own .scss/.css file
```angular2
@Component({
  selector: 'app-component',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})

```


### ViewEncapsulation is set on Native or Default, all global styles should be imported inside of the .scss files
> Bad
```angular2
@Component({
  selector: 'app-component',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  encapsulation: ViewEncapsulation.None
})
```
> Good
```angular2
@Component({
  selector: 'app-component',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
```
```scss 
/* app.component.scss */

@import  'path/to/scss/file';

// Use it here
```
### Styles use BEM/OOCSS/Atomic style guide.

Unless your project is based on some UI Framework.
```html
<aside class="sidebar">
    <div class="sidebar__top top">
        <div class="top__search-box">
        
        </div>
        <!-- Some Logo Here -->
    </div>
    <div class="sidebar__center center">
        <div class="center__list-of-fields">
        
        </div>
        <!-- Some Navigation Here -->
    </div>
    <div class="sidebar__bottom">
        <!-- Some Actions Here -->
    </div>
</aside>




```


```scss
.sidebar { 
  // Styles
    &__top {
      // Styles 
    }
    &__center {
      // Styles
    }
    &__bottom {
      // Styles
    }  
}

.top{
  &__searchBox {
    // Styles
  }
}

.center { 
  &__listOfFields {
    // Styles  
  } 
}
```

Other:
* Angular Material
* Bootstrap
* Tailwindcss
* Others


### All framework styles are imported in main `styles.scss/.css` files
```scss
// style.scss (in root)

@import "~bootstrap/dist/css/main.css";
```
### Class names are understandable
> Bad
```scss
.someAweSomeButton {
  // Styles   
}
```
> Good
```scss
.submit-button {
  // Styles 
}
```
### Do not query by Tag names
> Bad
```scss
    a {
      // Styles 
    }

    p {
      // Styles
    }
```
> Good
```scss
    .link {
      // Styles
    }  
    .article-paragraph {
      // Styles      
    }
```

### Awoid to tested styles
> Bad
```scss
    .link {
      .link-url{
        .link-button {
         
        }         
      }   
    } 
```
If you have this kind of nestings. That means you need to fix your HTML Classes.

### Style modules should be as easy as it could be
It's really important to keep our styles as easy and dummy as it could be. 

### Remember to use mixins, variables, parent relation (&)
> Bad
```scss
.card-top-info {
  display: flex;
  flex-direction: column;

  .card-top-info__top {
    display: flex;
    align-items: center;
    margin-bottom: 10px;

    .card-top-info__top__key {
      flex: 1;
      font-size: $font-size-h2;
      font-weight: $font-lite;
      color: $font-light-color;
    }

    .card-top-info__top__value {
      font-size: $font-size-h1;
      font-weight: $font-bold;
    }
  }
}
```
> Good
```scss
.card-top-info__top {
  display: flex;
  flex-direction: column;

  &_top {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }

  &__top-value {
      font-size: $font-size-h1;
      font-weight: $font-bold;
  }

  &__top-key {
      font-size: $font-size-h2;
      color: $font-light-color;
  }
}
```

### Media should be styled inside of the class
> Bad
```scss
@media (min-width: 320px) and (max-width: 991px) {
    .class1 {
    ...
    }
    .class2 {
    ...
    }
}
```
> Good
```scss
  .class1 {
    ...
    @media (min-width: 320px) and (max-width: 991px) {
    // Styles goes here
    }
  }
```

More: https://github.com/airbnb/css

## RxJS <a name="rxjs"></a><a name="6.0"></a>[6.0](#rxjs)

### Unsubscribe

To avoid “memory leak” each component should unsubscribe all subscriptions in ngOnDestroy hook

```javascript
someSubscription1 : Subscription;
someSubscription2 : Subscription;

ngOnInit() {
  this.someSubscription1 = ...subscribe(...);
  this.someSubscription2 = ...subscribe(...);
}

ngOnDestroy() {
  if (this.someSubscription1) {
    this.someSubscription1.unsubscribe();
  }

  if (this.someSubscription2) {
    this.someSubscription2.unsubscribe();
  }

}
```

We can easily manage it with:

```javascript
someSubscriptions : new Subscription();

ngOnInit() {
  this.someSubscriptions.add(...subscribe(...));
  this.someSubscriptions.add(...subscribe(...));
}
ngOnDestroy() {
  this.someSubscriptions.unsubscribe();
}
```

### Async Pipe

Recommended way to handle Observables in HTML is using async pipes provided by angular.
Async pipe automatically unsubscribe if component is destroyed.

```javascript

// Component code

public dataToDisplay$: Observable<Array<DataType>>
constructor(public readonly service: Service){}

public ngOnInit(): void {
  this.dataToDisplay$ = this.service.getData();
}
```

```html
<ul class="list">
	<li class="list__element" *ngFor="let element of dataToDisplay$ | async">
		{{element?.name}}
	</li>
</ul>
```

> Note: Always use trackBy function to avoid duplications or unnecessary re-rendering elements <a name="performance--trackBy"></a><a name="9.1">
> More </a>

## Immutable <a name="immutable"></a><a name="7.0"></a>[7.0](#immutable)

Basically it comes down to the fact that immutability increases predictability, performance (indirectly) and allows for mutation tracking.

**Predictability**

Mutation hides change, which create (unexpected) side effects, which can cause nasty bugs. When you enforce immutability you can keep your application architecture and mental model simple, which makes it easier to reason about your application.

**Performance**

Even though adding values to an immutable Object means that a new instance needs to be created where existing values need to be copied and new values need to be added to the new Object which cost memory, immutable Objects can make use of structural sharing to reduce memory overhead.
All updates return new values, but internally structures are shared to drastically reduce memory usage (and GC thrashing). This means that if you append to a vector with 1000 elements, it does not actually create a new vector 1001-elements long. Most likely, internally only a few small objects are allocated.
You can read more about this here.

**Mutation Tracking**

Besides reduced memory usage, immutability allows you to optimize your application by making use of reference- and value equality. This makes it really easy to see if anything has changed. For example a state change in a react component. You can use shouldComponentUpdate to check if the state is identical by comparing state Objects and prevent unnecessary rendering. You can read more about this here.

**Bad**

```javascript
const person = {
	name: "Frank",
	age: "22",
};

person.name = "Will"; // DONT!
```

**Good**

```javascript
const updated_person = {
	...person,
	name: "Will", // CORRECT
};
```

## Types and Interfaces <a name="types-and-interfaces"></a><a name="8.0"></a>[8.0](#types-and-interfaces)

All types and interfaces should be stored in one place (example `src/interfaces/`) to provide SSOT (single source of truth) and use public API to avoid long import paths.

TSLint should take care of the rest.

## Performance <a name="performance"></a><a name="9.0"></a>[9.0](#performance)

### TrackBy Function <a name="performance--trackBy"></a><a name="9.1"></a>[9.1](#performance--trackBy)

Imagine that we are trying to display an editable table with many rows. In case of changing single row, angular has to re render all DOM (component related) again.

```html
<table>
	<tr *ngFor="let car of cars; trackBy: trackByCarId">
		<td>{{ car.id }}</td>
	</tr>
</table>
```

```javascript
trackByCarId(index: number, car: Car) {
  return car.id; // or index
}
```

### Update onBlur <a name="performance--onBlur"></a><a name="9.2"></a>[9.2](#performance--onBlur)

Angular by default runs validation process every time when FormControl value changes.
Many callbacks run because of single letter in the input. We can easily run our validation onBlur.

```javascript
this.email = new FormControl(null, { updateOn: "blur" });
```

We can also run our validation after submitting the form with `updateOn: submit`.

Now Angular can follow only changes by unique identifier and destroy / build elements without re rendering elements that didn’t change.

### @Input() setter <a name="performance--input-setter"></a><a name="9.2"></a>[9.2](#performance--input-setter)

Angular share a ngOnChanges hook that runs every time Input() gets a new value. The problem is, we usually have many inputs, which in this case it’s not optimal. If we want to run some code based on the input we should do it via setter.

```javascript
@Input() set someInput(value) {
  this.runSomeFunction();
}
```

**IMPORTANT**
In the case of relations between the imports, it’s very important to pay attention on ordering them in the correct way.

### Change detection Ref <a name="performance--change-detection-ref"></a><a name="9.3"></a>[9.3](#performance--change-detection-ref)

Angular allows to manually detect changes in component. ChangeDetectorRef is usually used to detect changes along with the OnPush strategy. ChangeDetectorRef has a method for it `detach();` which detach component from the ChangeDetectors tree.

```javascript
constructor(private readonly changeDetector: ChangeDetectorRef) {
  this.changeDetector.detach();
}
```

We can also manually run detection system at any moment in the component with `detectChanges()` method (it checks components and his children) or `markForCheck()` (it checks all components from root until this component).

### Change Detection Strategy <a name="performance--change-detection-strategy"></a><a name="9.4"></a>[9.4](#performance--change-detection-strategy)

Most important method to increase an application performance. In shortcut, change detection will be triggered only if Input() object reference would change or component would emit some event. It’s perfect to use in “dummy” components - presentation only.

```javascript
@Component({
  ...
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

### Testing

To avoid bugs while developing features important is to write Unit & Integrations tests.

Instead of using build in config we can easily use
a NPM package (https://github.com/ngneat/spectatorr)

Instead of doing this:

```typescript
describe("MyComponent", () => {
	beforeEach(async => {
		return TestBed.configureTestModule({
			imports: [SomeModuleWithDependencies],
			declarations: [
				TestHostComponent,
				MyComponent, // <-- All I want to do is test this!!
				// We either must list all our dependencies here
				// -- OR --
				// Use NO_ERRORS_SCHEMA which allows any HTML to be used
				// even if it is invalid!
				ButtonComponent,
				LinkComponent,
				FooDirective,
				BarPipe,
			],
			providers: [MyService],
		})
			.compileComponents()
			.then(() => {
				let myService = TestBed.get(MyService); // Not type safe
				spyOn(myService, "foo").and.returnValue("mocked foo");
			});
	});

	it("renders a link with the provided label text", () => {
		const fixture = TestBed.createComponent(TestHostComponent);
		fixture.componentInstance.labelText = "my text";
		fixture.detectChanges();
		const link = fixture.debugElement.query(By.css("a"));

		expect(a.nativeElement.innerText).toBe("my text");
	});

	it('sends "foo" to bound click events', () => {
		const fixture = TestBed.createComponent(TestHostComponent);
		spyOn(fixture.componentInstance, "handleClick");
		fixture.detectChanges();
		const myComponentElement = fixture.debugElement.query(
			By.directive(MyComponent)
		);
		myComponentElement.click();

		expect(fixture.componentInstance.handleClick).toHaveBeenCalledWith("foo");
	});
});

@Component({
	template: `
    <my-component [linkText]="linkText" (click)="handleClick($event)">
    </my-component>
  `,
})
class TestHostComponent {
	linkLabel: string;
	handleClick() {}
}
```

we can easily do this

```typescript
import { Spectator, createComponentFactory } from '@ngneat/spectator';
import { ButtonComponent } from './button.component';

describe('ButtonComponent', () => {
  let spectator: Spectator<ButtonComponent>;
  const createComponent = createComponentFactory(ButtonComponent);

  beforeEach(() => spectator = createComponent());

  it('should have a success class by default', () => {
    expect(spectator.query('button')).toHaveClass('success');
  });

  it('should set the class name according to the [className] input', () => {
    spectator.setInput('className', 'danger');
    expect(spectator.query('button')).toHaveClass('danger');
    expect(spectator.query('button')).not.toHaveClass('success');
  });
});
```


#### It doesn't matter what approach will you choose. Important is to provide best code quality and code coverage.

### Each component should have at least Unit tests to cover all methods.

> **We expect to have at least 80-85% test coverage.**

## PWA 

> source
> https://angular.io/guide/service-worker-intro > https://developers.google.com/web/progressive-web-apps/

Angular applications, as single-page applications, are in a prime position to benefit from the advantages of service workers. Starting with version 5.0.0, Angular ships with a service worker implementation. Angular developers can take advantage of this service worker and benefit from the increased reliability and performance it provides, without needing to code against low-level APIs.

### Create an installable PWA

To make your Angular application a PWA, all you need to do is run a single command:

```bash
ng add @angular/pwa
```

This command will:

Create a service worker with a default caching configuration.
Create a manifest file, which tells the browser how your app should behave when installed on the user's device.
Add a link to the manifest file in `index.html.`
Add the theme-color `<meta>` tag to `index.html.`
Create app icons in the `src/assets` directory.

### Precache

https://web.dev/precaching-with-the-angular-service-worker/

## License

Copyright (c) 2020 Franciszek Stodulski

**[⬆ back to top](#table-of-contents)**
