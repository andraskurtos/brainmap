---
created: 20240901 23:53
tags:
  - angular
  - cs
cssclasses: []
---

# Angular components

Angular components are the basic building blocks of Angular applications.

Each component has 3 parts:
- [[TypeScript]] Class
- [HTML template](HTML template)s
- [[CSS]] styles

## Hello World!

Example of a Hello World site in Angular:

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    Hello World
  `,
  styles: `
    :host {
      color: blue;
    }
  `,
  standalone: true,
})
export class AppComponent {}
```

The 'template' section is the [[HTML template]], and the 'styles' section is the CSS styles inside the [[TypeScript]] class.

## Component Class
The component's logic and behavior is defined in the [[TypeScript]] class.

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    Hello {{ city }}
  `,
  standalone: true,
})
export class AppComponent {
  city = 'San Francisco'
}

```

In the snippet above, we have added class property to the [[TypeScript|TS]] class, storing a string. The type can be omitted because of type interference in [[TypeScript]].

To use a class property in the HTML template, we use the syntax `{{ property }}`. 

You can also do other operations with interpolation, such as `{{ 1+1 }}`. Angular will evaluate the contents and render accordingly.

## Selector property
The selector property is used when referencing the component in another template. You can use it like any other HTML tag, e.g.
`<app-user />`.

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-user',
  template: `
    Username: {{ username }}
  `,
  standalone: true,
})
export class UserComponent {
  username = 'youngTech';
}

@Component({
  selector: 'app-root',
  template: `<section><app-user/></section>`,
  standalone: true,
  imports: [UserComponent],
})
export class AppComponent {}

```

In this example, we are referencing the UserComponent component from the AppComponent. For this, we need to import it in the `imports` section, then use the HTML tag `<app-user/>`.

# heading

