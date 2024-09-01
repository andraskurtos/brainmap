---
created: 20240901 23:53
tags:
  - angular
  - html
  - cs
cssclasses:
---

# Angular HTML templates
HTML templates are one of three building blocks of Angular components. They are used mainly to control what is displayed on screen.
## Referencing other components with Selector Property:
You can use the `selector:` property of a component as an HTML tag inside a template to reference it. E.g. `<app-user / >`.

## Conditions
You can express conditional displays in templates using the `@if` syntax.

```ts
@Component({
  ...
  template: `
    @if (isLoggedIn) {
      <p>Welcome back, Friend!</p>
    }
  `,
})
class AppComponent {
  isLoggedIn = true;
}
```

This snippet will display 'Welcome back, Friend!' if the *isLoggedIn* property of the TypeScript class is *true*.

## For loops
You can create forloops in templates using the `@for` syntax.
```ts
@Component({
  ...
  template: `
    @for (os of operatingSystems; track os.id) {
      {{ os.name }}
    }
  `,
})
export class AppComponent {
  operatingSystems = [{id: 'win', name: 'Windows'}, {id: 'osx', name: 'MacOS'}, {id: 'linux', name: 'Linux'}];
}
```

This snippet loops through the list defined in the TS class and displays every os' name.


