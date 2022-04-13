# Angular Component Class

### In this class you will learn:

* How to use the command line to create a component;
* How to apply a global style and at component level;
* How to pass data into a component;
* How to emit event from a component and listening to it;
* How use content projection;
* How to select content to be projected;

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

To finish this part, call the component on the app component `.html` file.
