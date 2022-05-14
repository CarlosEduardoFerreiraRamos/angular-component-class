## Dependency Injection Class

### In this class you will learn:

- How to use the command line to create a service;
- How to provide a value;
- How to access the provided value;
- How to overwrite the provided value;
- How to pass data to different scopes;
- How to control the accessed value origin;

### Prerequisites:

Open a terminal at the root of this project and install the packages.

```
npm install
```

### Create a service

First, let's build a user service so we can study the concepts of the class. On a terminal navigate to the app folder inside the `dependency-injection-class` folder. There, run the angular client command to generate a service for your application. Let's call the service `User`, so the last argument should be `user`.

```
ng generate service <service-name>
```

After the command is executed a file called `user.service.ts` should be created in the `app` folder. The file contains instructions for a class that will be provided at some level of the injection tree. Add a attribute call `serviceValue` to the service, and add the following value to it `from User service`.

To finish this part, lets call the `UserService` in our `app.component.ts` , by calling it in the component constructor and overwriting the attribute `title` value with the service `serviceValue` attribute. The new value should appear on the view.

```ts
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class UserService {
  public serviceValue = "from User service";

  constructor() {}
}
```

```ts
export class AppComponent {
  title = "dependency-injection-class";

  constructor(private _userService: UserService) {
    this.title = this._userService.serviceValue;
  }
}
```

### Provide a Value

This was kindof tuch in previus topic.
