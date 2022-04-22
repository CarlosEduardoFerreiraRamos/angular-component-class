## Angular Component Class

### In this class you will learn:

- How to use the command line to create a component;
- How to apply a style at component level;
- How to pass data into a component;
- How to emit event from a component and listening to it;
- How use content projection;
- How to select content to be projected;

### Pre requists:

Open a terminal at the root of this project and install the packages.

```
npm install
```

#

### Create the component:

First, let's build a card component so we can study the concepts of the class. On a terminal navigate to the app folder. There, run the angular client command to generate a component for your application. Let's call the component `Card`, so the last argument should be `card`.

```
ng generate component <component-name>
```

or

```
ng -g -c <component-name>
```

After the command is executed a folder called `card` with three files insed should be created in the `app` folder. Each file contains the instroctions for part of the component. The `.hmlt` contains the view, the `.scss|.sass|.css` contains the component styles, and the `.ts` the components class and metada.

Also, the Component will be added at the app module in the declarions array.

To finish this part, call the component on the app component `.html` file, by writting the component selector. The selector you should use can be found in the component declaration file.

```ts
// card.component.ts
@Component({
  selector: 'app-card',
  ...
})
return class CardComponent {}
```

```html
<!-- app.component.html -->
<app-card></app-card>
```

A text `component card works` should appear in hosted page.

#

### Style the component

Our card component is created. But it doesn't look like a card at all. So let's add some styles like a border.

First we have to talk about the types of css preprocessor that come with Angular out of the box. These preprocessors transform your written code in css files that can read by the browser. Withing the framework comes `sass`, `less`, and `scss`. In most projects that I worked on `scss` was used, and this is what I would recomend.

Like other MVC frameworks in Angular the component styles are separeted from the rest, being declered in the `<component-name>.component.scss`. This file is pointed in the components' metadata. The properties used are the `styleUrls`, that receives an array of string, each the url of a style file, and the `styles`, that receives an array of string, each a style declaration.

```ts
// card.component.ts
@Component({
  selector: 'app-card',
  styleUrls: ['./card.component.scss'],
  ...
})
return class CardComponent {}
```

or

```ts
// card.component.ts
@Component({
  selector: 'app-card',
  styles: [`
    .any-class {
      display: block;
    }
  `],
  ...
})
return class CardComponent {}
```

Now, knowing all that let's add a style to our card component.

First lets give our component a more "Card" look by adding a border. To select the component it self use the `:host` selector.

```scss
// card.component.scss
:host {
  display: block;
  border: 1px solid black;
  border-radius: 6px;
}
```

If you want add some more styles to our card.

#

### Passing data into a component

Whit our component all pritty and fancy we can now focus in its functionalities. Our component being a card we should allow to a value to be rendered in its body. To achieve this we will use the `Input` decorator to pass a string.

In our component lets create a attribute called `text`, a `string`, that will have a special Angular decorator `@`, the `Input`. This will tell Angular that this component has a `prop` with the name `text`, and will allow us to call this attribute in the view.

```ts
// card.component.ts
import { Input } from "@angular/core";

return class CardComponent {
  @Input() text: string;
};
```

Now we will replace the original template `component card works` with the name of our attribute.

```html
<!-- card.component.html -->
{{ text }}
```

And now we can pass any value to our component in the app component template.

```html
<!-- app.component.html -->
<app-card text="Any value"></app-card>
```

#

### Emitting events from a component and listening to it

Now with our component allowing more customization we may want listen to a specific inside the component. We can achieve this by creating another attribute, called `press`, that will have a special Angular decorator `@`, the `Output`. This will tell Angular that this component has a `event` with the name `press`, and will allow us to call this attribute in the view. Also this attribute must be initiated using the `EventEmitter` object. And for last let call the `press` attribut in a method, the `onPress`, emitting the event.

```ts
// card.component.ts
import { Input, Output } from "@angular/core";

return class CardComponent {
  @Input() text: string;

  @Output() press = new EventEmitter<string>();

  public onPress(): void {
    this.press.emit("From card with text: ", this.text);
  }
};
```

We also must call the new method in our card view.

```html
<!-- card.component.html -->
<div (click)="onPress()">{{ text }}</div>
```

And now we can listen to this event in the app component. First lets create a function in the app component to be called in the app view.
This function receives the `value` parameter, a `string`, and use to call an `alert`.

```ts
export class AppComponent {
  public onPressCard(value: string): void {
    alert(value);
  }
}
```

To listen our custom `press` event in the card component we can use the same syntax used in the card template. The only difference is the `$event` used to capture the value passed by the event.

```html
<!-- app.component.html -->
<app-card text="Any value" (press)="onPressCard($event)"></app-card>
```

#

### Content Projection

Content projection, or transclusion in Angular, is the feature that allows us to project a part of a view, in the content of a component, on a parent component view. It is a very powerfull tool, and a simple one to use.

For exemple, in our `Card` component add a `ng-content` tag to its view.

```html
<!-- card.component.html -->
<div (click)="onPress()">{{ text }}</div>

<!-- child content will be projected here -->
<ng-content></ng-content>
```

And in our app component write a text or any other element in the `card-component` content.

```html
<!-- app.component.html -->
<app-card text="Any value" (press)="onPressCard($event)">
  <div>Projected content</div>
</app-card>
```

Written this way the projected content will appear bellow the text passed through the input. We can change the position of the `ng-content` and that will change the position of the projected content. Take note, that if the view contains multiple `ng-content` tags the content will not be projected multiple times. Insted the content will be projected in the position of the last `ng-content` tag. The only case to use multiple `ng-content` tags would be when selecting especific content to be projected.

#

### Selecting Content Projection

The hability to select the content to be project is a powerfull one. The `ng-content` tag has the `select` property that uses the css selector pattern to target elements in the component content to be projected.

For exemple, in our `Card` component add a `ng-content` tag before the `div` element with the text attribute. In it add a `select` property that target an element with the `card_header` property.

```html
<!-- card.component.html -->

<!-- child content with the card_header prop will be projected here -->
<ng-content select="card_header"></ng-content>

<div (click)="onPress()">{{ text }}</div>

<!-- child content will be projected here -->
<ng-content></ng-content>
```

And in our app component write a text or any other element in the `card-component` content with the `card_header` property.

```html
<!-- app.component.html -->
<app-card text="Any value" (press)="onPressCard($event)">
  <strong card_header>Projected Header</strong>
  <div>Projected content</div>
</app-card>
```

This way, the elements selected will be projected in their especific `ng-content` tag, and all the others in the general one.

#

## Chalenges

#

### Create a Card component

- Create a card component using angular CLI;
- The component must receive three values as inputs, a header, body, and footer;
- The component must display the three previous values in its view;
- The component must emit an event when one of its values is clicked;
- The component must emit three diferente events, one for each of its values when these are clicked;
- The component must render its child content in its view;
- The component must render especific child contents in its header, body, and footer;
