## Angular Component Class

### In this class you will learn:

- How to use the command line to create a component;
- How to apply a style at the component level;
- How to pass data into a component;
- How to emit an event from a component and listen to it;
- How to use content projection;
- How to select content to be projected;

### Prerequisites:

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

After the command is executed a folder called `card` with three files inside should be created in the `app` folder. Each file contains the instructions for a part of the component. The `.hmlt` contains the view, the `.scss|.sass|.css` contains the component styles, and the `.ts` the component's class and metadata.

Also, the Component will be added to the app module in the declarations array.

To finish this part, call the component on the app component `.html` file, by writing the component selector. The selector you should use can be found in the component declaration file.

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

A text `card works` should appear on the hosted page.

#

### Style the component

Our card component is created. But it doesn't look like a card at all. So let's add some styles.

First, we have to talk about the types of CSS preprocessors that come with Angular out of the box. These preprocessors transform your written code into CSS files that can be read by the browser. Within the framework comes `sass`, `less`, and `scss`. We will use `scss`.

The component styles are separated from the rest in the `<component-name>.component.scss`. This file is pointed in the components' metadata. The properties used are the `styleUrls`, which receives an array of string, each the URL of a style file, and the `styles`, which receives an array of string, each a style declaration.

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

Let's give our component a more "Card" look by adding a border. To select the component itself use the `:host` selector.

```scss
// card.component.scss
:host {
  display: block;
  border: 1px solid black;
  border-radius: 6px;
}
```

If you want, add some more styles to our card component.

#

### Passing data into a component

In our component let's create an attribute called `text`, a `string`, and will allow us to call this attribute in the view. Also, this attribute will have a special Angular decorator `@`, the `Input`. This will tell Angular that this component has a `prop` with the name `text`.

```ts
// card.component.ts
import { Input } from "@angular/core";

return class CardComponent {
  @Input() text: string;
};
```

Now we will replace the original template `card works` with the name of our attribute.

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

In our component let's create another attribute, called `press`, that will have a special Angular decorator `@`, the `Output`. This will tell Angular that this component has an `event` with the name `press`, and will allow us to call this attribute in the view. Also, this attribute must be initiated using the `EventEmitter` object. And for the last let's call the `press` attribute in a method, the `onPress`, emitting the event.

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

Now we can listen to this event in the app component. First, let us create a function in the app component to be called in the app view.
This function receives the `value` parameter, a `string`, and use to call an `alert`.

```ts
export class AppComponent {
  public onPressCard(value: string): void {
    alert(value);
  }
}
```

To listen to our custom `press` event in the card component we can use the same syntax used in the card template. The only difference is the `$event` used to capture the value passed by the event.

```html
<!-- app.component.html -->
<app-card text="Any value" (press)="onPressCard($event)"></app-card>
```

#

### Content Projection

Content projection, or transclusion in Angular, is the feature that allows us to project a part of a view, in the content of a component, on a parent component view. It is a very powerful tool, and a simple one to use.

For example, in our `Card` component add an `ng-content` tag to its view.

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

With the `ng-content` tag written this way the projected content will appear below the text passed through the input. We can change the position of the `ng-content` and that will change the position of the projected content. Take note, that if the view contains multiple `ng-content` tags the content will not be projected multiple times. Instead, the content will be projected in the position of the last `ng-content` tag. The only case to use multiple `ng-content` tags would be when selecting specific content to be projected.

#

### Selecting Content Projection

The ability to select the content to be projected is a powerfull one. The `ng-content` tag has the `select` property that uses an CSS selector pattern to target elements in the component content to be projected.

For example, in our `Card` component add an `ng-content` tag before the `div` element with the text attribute. In it add a `select` property that target an element with the `card_header` property.

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

This way, the elements selected will be projected in their specific `ng-content` tag, and all the others in the general one.

#

## Challenges

#

### Create a Card component

- Create a card component using angular CLI;
- The component must receive three values as inputs, a header, body, and footer;
- The component must display the three previous values in its view;
- The component must emit an event when one of its values is clicked;
- The component must emit three different events, one for each of its values when these are clicked;
- The component must render its child content in its view;
- The component must render specific child contents in its header, body, and footer;

### Special Challenge: Content Overwrites Inputs Values

- Create a logic that hides the values from the input when content is projected.

`This can be done using CSS selector or programmatically`
