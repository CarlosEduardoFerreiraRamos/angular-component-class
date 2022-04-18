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

### How to pass data into a component
