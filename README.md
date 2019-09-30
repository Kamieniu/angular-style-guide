# Angular Style Guide

*A mostly reasonable approach to build Angular applications*

> **Note**: These rules are only (in my opinion) the best way to keep your code semantic, clean and readable.

## Table of Contents

  1. [Files Structure and Name Conventions](#files-structure-and-name-conventions)
    1.1 [Components](#components)
    1.2 [Containers](#containers)
    1.3 [Views](#views)
    1.3 [Modules](#modules)
  1. [Creating new blocks - Angular CLI](#creating-new-blocks-angular-cli)
  1. [Public API](#public-api)
  1. [HTML Templates](#html-templates)
  1. [Styles](#styles)
  1. [Styling Name Convention](#styling-name-convention)
  1. [Types and Interfaces](#types-and-interfaces)
  1. [RxJS](#rxjs)
  1. [Performance](#performance)
  1. [Testing](#testing)
  1. [PWA](#PWA)
  1. [Services](#services)
  1. [TSLint](#tslint)
  1. [Classes & Constructors](#classes--constructors)
  1. [NgModules](#NgModules)

  ## Files Structure and Name Conventions <a name="files-structure-and-name-conventions"></a><a name="1.1"></a>[1.1](#files-structure-and-name-conventions) 
  ### Structure 
  Files Structure should be divided, clear and understandable.
  
  - [1.1](#components) **Components**:
  It's a simple, single block of reusable (or not) code that represents some UI and logic. The best way is to keep our  components "dummy" - not connected to the API, services, store, etc. All data should be served by parents (containers)
  
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
   - [1.2](#containers) **Containers**:
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
  
  - [1.3](#views)**Views**:
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
  
  ### Services
  
  ```
    |- Services
    |-- [service-name]
    |--- [service-name].service.ts
    |--- [service-name].service.spec.ts
    ...
    |-- index.ts (public api export)
  ```
  ### Pipes
  
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
  ### Interfaces
  
  ```
    |- Interfaces
    |-- [interface-name]
    |--- [interface-name].interface.ts
    ...
    |-- index.ts (public api export)
  ```
  
  **Export**: All files should be exported by `index.ts`. 
  **Access**:`tsconfig.json` should have applied aliases for paths.
  
    ```json
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
    ```

**[⬆ back to top](#files-structure-and-name-conventions)**

## Creating new blocks - Angular CLI

  <a name="rcreating-new-blocks-angular-cli"></a><a name="2.1"></a>
  - [2.1](#creating-new-blocks-angular-cli) Angular CLI is a powerfull tool. It does not only accelerates our work but also keep all files in the same "shape".

    > To generate component/group in our file structure

    ```
    ng g m components/buttons
    ng g c components/buttons/defaultButton -t --style=scss
    ```
    
    > To generate containers in our file structure

    ```
    ng g m containers/buttons
    ng g c components/buttons/defaultButton -t --style=scss
    ```
    

**[⬆ back to top](#table-of-contents)**

## Objects

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) Use the literal syntax for object creation. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties) Use computed property names when creating objects with dynamic property names.

    > Why? They allow you to define all the properties of an object in one place.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand) Use object method shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) Use property value shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > Why? It is shorter and descriptive.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) Group your shorthand properties at the beginning of your object declaration.

    > Why? It’s easier to tell which properties are using the shorthand.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) Only quote properties that are invalid identifiers. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

    ```javascript
    // bad
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // good
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - [3.7](#objects--prototype-builtins) Do not call `Object.prototype` methods directly, such as `hasOwnProperty`, `propertyIsEnumerable`, and `isPrototypeOf`. eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

    > Why? These methods may be shadowed by properties on the object in question - consider `{ hasOwnProperty: false }` - or, the object may be a null object (`Object.create(null)`).

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key));

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
    console.log(has.call(object, key));
    /* or */
    import has from 'has'; // https://www.npmjs.com/package/has
    console.log(has(object, key));
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) Prefer the object spread operator over [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
    delete copy.a; // so does this

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) Use the literal syntax for array creation. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) instead of direct assignment to add items to an array.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) Use array spreads `...` to copy arrays.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="4.4"></a>
  - [4.4](#arrays--from-iterable) To convert an iterable object to an array, use spreads `...` instead of [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // good
    const nodes = Array.from(foo);

    // best
    const nodes = [...foo];
    ```

  <a name="arrays--from-array-like"></a>
  - [4.5](#arrays--from-array-like) Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) for converting an array-like object to an array.

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // bad
    const arr = Array.prototype.slice.call(arrLike);

    // good
    const arr = Array.from(arrLike);
    ```

  <a name="arrays--mapping"></a>
  - [4.6](#arrays--mapping) Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) instead of spread `...` for mapping over iterables, because it avoids creating an intermediate array.

    ```javascript
    // bad
    const baz = [...foo].map(bar);

    // good
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.7](#arrays--callback-return) Use return statements in array method callbacks. It’s ok to omit the return if the function body consists of a single statement returning an expression without side effects, following [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => x + 1);

    // bad - no returned value means `acc` becomes undefined after the first iteration
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
    });

    // good
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.8](#arrays--bracket-newline) Use line breaks after open and before close array brackets if an array has multiple lines

    ```javascript
    // bad
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // good
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ back to top](#table-of-contents)**


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
