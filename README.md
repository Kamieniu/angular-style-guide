# Angular Style Guide

> A mostly reasonable approach to build Angular applications
> **Note**: These rules are only (in my opinion) the best way to keep your code semantic, clean and readable.

## Table of Contents

1. [Files Structure and Name Conventions](#files-structure-and-name-conventions):
2. [Creating new blocks - Angular CLI](#creating-new-blocks-angular-cli)
3. [Public API](#public-api)
4. [HTML Templates](#html-templates)
5. [Styles](#styles)
6. [Styling Name Convention](#styling-name-convention)
7. [Types and Interfaces](#types-and-interfaces)
8. [RxJS](#rxjs)
9. [Performance](#performance) 10. [Testing](#testing)
10. [PWA](#PWA)
11. [TSLint](#tslint)
12. [Classes & Constructors](#classes--constructors)

## Files Structure and Name Conventions <a name="files-structure-and-name-conventions"></a><a name="1.1"></a>[1.1](#files-structure-and-name-conventions)

### Structure

Files Structure should be divided, clear and understandable.

- <a name="files-structure-and-name-conventions--components"></a><a name="1.1"></a>[1.1](#files-structure-and-name-conventions--components) **Components**:
  It's a simple, single block of reusable (or not) code that represents some UI and logic. The best way is to keep our components "dummy" - not connected to the API, services, store, etc. All data should be served by parents (containers)

```
  |- Components
  |-- [component-name]
  |--- [component-name].component.ts
  |--- [component-name].component.spec.ts
  |--- [component-name].component.scss
  |--- [component-name].module.ts
  ... or //
  |-- [group-name]
  |---- [component-name].component.ts
  |---- [component-name].component.spec.ts
  |---- [component-name].component.scss
  |--- [group-name].module.ts
  |-- index.ts (public api export)
  |
```

- <a name="files-structure-and-name-conventions--containers"></a><a name="1.2"></a>[1.2](#files-structure-and-name-conventions--containers) **Containers**:
  Containers are wrappers for components. They provice all API connections, pass data, etc.

Examples:

- forms
- gallery
- articles + images

```
  ...
  |- Containers
  |-- B-name
  |--- b-name.component.ts
  |--- b-name.component.spec.ts
  |--- b-name.component.scss
  |--- b-name.module.ts
  |-- B-name
  |--- b-name.component.ts
  |--- b-name.component.spec.ts
  |--- b-name.component.scss
  |--- b-name.module.ts
  |-- index.ts (public api export)
  |
  ...
```

- <a name="files-structure-and-name-conventions--views"></a><a name="1.3"></a>[1.3](#files-structure-and-name-conventions--views) **Views** 👀:

  Views are representation of each page visible for user (connected with routes and/or lazy loading modules). Views are wrapper for containers/components and provide most important APIs connections.

```
  |- views
  |-- [page-name]-view
  |--- [page-name]-routing.module.ts
  |--- [page-name].component.ts
  |--- [page-name].component.spec.ts
  |--- [page-name].component.scss
  |--- [page-name].module.ts
  |-- [page-name]-view
  |--- [page-name]-routing.module.ts
  |--- [page-name].component.ts
  |--- [page-name].component.spec.ts
  |--- [page-name].component.scss
  |--- [page-name].module.ts
  |-- index.ts (public api export)
```

- <a name="files-structure-and-name-conventions--modules"></a><a name="1.4"></a>[1.4](#files-structure-and-name-conventions--modules) **Modules**:

  Modules are boxes 📦 for some piece of code. It allows to share component declaraions, pipes, modules between modules in the application.

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

- <a name="files-structure-and-name-conventions--services"></a><a name="1.5"></a>[1.5](#files-structure-and-name-conventions--services) **Services**:
  Services are usually used to consume some API, deliver some data or share data between components (not recommended - use Store instead)

```
  |- Services
  |-- [service-name]
  |--- [service-name].service.ts
  |--- [service-name].service.spec.ts
  ...
  |-- index.ts (public api export)
```

- <a name="files-structure-and-name-conventions--pipes"></a><a name="1.6"></a>[1.6](#files-structure-and-name-conventions--pipes) **Pipes**:
  Filters, sanitizers for our code.

```
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

```
  |- Interfaces
  |-- [interface-name]
  |--- [interface-name].interface.ts
  ...
  |-- index.ts (public api export)
```

**Export**: All files should be exported by `index.ts`.
**Access**:`tsconfig.json` should have applied aliases for paths.

````javascript
    ...
    "paths: {
      "app/components": ["src/components/index.ts"],
      "app/services": ["src/services/index.ts"],
      ...
    }
    ```

    so later on all files are accessable by

    ``` javascript
    import { ServiceA } from 'app/services'
    ....
````

**[⬆ back to top](#files-structure-and-name-conventions)**

## Creating new blocks - Angular CLI <a name="creating-new-blocks-angular-cli"></a><a name="2.0"></a>[1.1](#creating-new-blocks-angular-cli)

Angular CLI is a powerfull tool. It does not only accelerates our work but also keep all files in the same "shape".

### To generate component/group in our file structure

```bash
ng g m components/buttons
ng g c components/buttons/defaultButton -t --style=scss
```

### To generate containers in our file structure

```bash
ng g m containers/buttons
ng g c components/buttons/defaultButton -t --style=scss
```

### To generate services in our file structure

> Unfortunately Angular CLI doesn't support creating folders for services. You need to do it manually

```bash
mkdir services/[service-purpose]
ng g s services/[service-name]
```

### To generate Interface in our file structure

```bash
ng g i interfaces/[interface-name]
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

**For the developer, however, there are a number of differences that you have to consider.**

You get better code completion and inline support in your editor/IDE in most cases when the HTML is in a separate .html file. (IntelliJ IDEA, at least, supports HTML for inline templates and strings)
There is a convenience factor to having code and the associated HTML in the same file. It's easier to see how the two relate to each other.
These two things will be of equal value for many people, so you'd just pick your favorite and move ahead with life if this were all there was to it.
But that leads us to the reasons you should keep your templates in your components, in my opinion:
It is difficult to make use of relative filepaths for external templates as it currently stands in Angular 2.
Using non-relative paths for external templates makes your components far less portable, since you need to manage all of the /where/is/my/template type references from the root that change depending on how deep your component is.
That's why I would suggest that you keep your templates inside your components where they are easily found. Also, if you find that your inline template is getting large and unwieldy, then it is probably a sign that you should be breaking your component down into several smaller components, anyway.

**My own opion is to keep Component Template as a inline html. This way we force ourself to keep component's code small.**

> If you use VScode to produce code I suggest to install this extension:
> https://marketplace.visualstudio.com/itemstemName=natewallace.angular2-inline
> that helps you to format HTML code in your .ts files

## License

(The MIT License)

Copyright (c) 2012 Franciszek Stodulski

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team’s style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };
