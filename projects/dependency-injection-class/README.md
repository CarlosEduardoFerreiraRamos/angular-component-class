## Dependency Injection Class

### In this class you will learn:

- How to use the command line to create a service;
- How to provide a value;
- How to access the provided value;
- How to overwrite the provided value;
- How to pass data to different scopes; (pending)

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

This was kindof tuch in previus topic. Just by creating a service with the `providedIn` in the Injectable metadata set to `root` will provide the the `UserService`, in a single context, to the entire application.

The `providedIn` property also accepts other values, such the declaration of a module, `any` that creates a context for every module, `platform` for every application. The advantage of using these values in the `providedIn` is that you are leaving to Angular to manage when the context is created and destroyed.

Another way to provide a value is to provide in the `providers` metadata property. It accepts a arrays of Injectables, especific objects, and functions. It can be found in `@Module`, `@Directive`, and in `@Component` metadata.

Let's change our `UserService` to be provided in the `AppModule`.

First remove the metadata from our `UserService`. It should be like this.

```ts
@Injectable()
export class UserService {
  public serviceValue = "from User service";

  constructor() {}
}
```

If you try to execute the development server now it will throw an error, because Angular will try to find a Injectable with the `UserService` token.

Now, at our `AppModule` add the `providers` property to the module metadata, the `NgModule`, providing the `UserService`.

```ts
@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, AppRoutingModule],
  providers: [UserService], // < - add here
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Ok, we just provide our service, an Injectable, using the `providers` property. But we can only provide Injectables in the `providers` property? The answare is no, we can provide EVERYTHING we want.

To provide something we first need a Identification Token. In the case of our service the class declaration was used as an Identifier, the `UserService`. So classes, not interfaces, can be used as tokens. But Angular provide a way to create a token without a class, using the `InjectionToken`.

So now, lets provide our `UserService` class using a `InjectionToken`. Create a `InjectionToken`, that it is a class and the constructor receives a string used as an identifier, do it anywhere like this.

```ts
import { InjectionToken } from "@angular/core";

export const APP_CONTEXT = new InjectionToken("app-context");
```

With our token created now we need to informe Angular that our `UserService` will be provided in the `APP_CONTEXT` token. To achieve this we have to pass a object in providers list. This object must have the `provide` property with the `APP_CONTEXT` as a value, and the `useClass` property with the `UserService` as a value.

```ts
@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, AppRoutingModule],
  providers: [{ provide: APP_CONTEXT, useClass: UserService }], // < - add here
  bootstrap: [AppComponent],
})
export class AppModule {}
```

We also have to change how we access the injected value in our `AppComponent`, adding the `APP_CONTEXT` in front of the property being called in the component constructor using the `@Inject` decorator.

```ts
export class AppComponent {
  title = "dependency-injection-class";

  constructor(@Inject(APP_CONTEXT) private _userService: UserService) {
    this.title = this._userService.serviceValue;
  }
}
```

By using the `APP_CONTEXT` token to provide the `UserService` class we don't need anymore the `Injectable` decorator on our service.

But if we don't want to provid a class, just a value? Well, insted of the property `useClass` we can use the `useValue`, and passa anything we want. In our case, lets pass an object simulating our `UserService`, an object with the `serviceValue` property.

```ts
@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, AppRoutingModule],
  providers: [
    {
      provide: APP_CONTEXT,
      useValue: { serviceValue: "Provided by the APP_CONTEXT token" },
    },
  ], // < - add here
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Extra method 1: Factory providers

```ts
provider: [{
  provide: HeroService,
  deps: [Logger, UserService],
  useFactory: (logger: Logger, userService: UserService) => {
    return new HeroService(logger, userService.user.isAuthorized);
  }
}]
```

Extra method 2: Injector Create

```ts
const Location = new InjectionToken('location');
const Hash = new InjectionToken('hash');

const injector = Injector.create({
  providers: [
    {provide: Location, useValue: 'https://angular.io/#someLocation'}, {
      provide: Hash,
      useFactory: (location: string) => location.split('#')[1],
      deps: [Location]
    }
  ]
});

expect(injector.get(Hash)).toEqual('someLocation');
```

### Access the Provided Value

This was kindof also viewed in previus topic. There is just one other way to access a provided vile by DI.

We could use the `Injector` service to access a value provided in the DI.

```ts
export class AppComponent {
  title = "dependency-injection-class";

  constructor(private _injector: Injector) {
    this.title = this._injector.get(UserService).serviceValue;
  }
}
```

So now we will see how to change the way injected value is accessed.

Normally Angular follows a very simple order to access the value. First it asks the host itself, them the parents, and the parents of the parents, intil it reachs its module, and there for into the root of the application.

To alterate the access Angular provides some decorators. They are `@Host`, `@Self`, `@SkipSelf`, and `@Optional`.

* `@Host` decorator says to Angular that access value can only fetch from the host element;

* `@Self` decorator says to Angular that access value can only fetch from itself;

* `@SkipSelf` decorator says to Angular that will not search for the access value in itself;

* `@Optional` decorator says to Angular teh value may not be present.

The decorators can be used when calling the provided value in a class contructor. The operators also can be chained to create complex rules. 

```ts
export class AppComponent {
  title = "dependency-injection-class";

  constructor(@SkipSelf() @Optional() private _userService: UserService) {
    this.title = this._userService.serviceValue;
  }
}
```

### Overwrite the Provided Value
