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

After the command is executed a file called `user.service.ts` should be created in the `app` folder. The file contains instructions for a class that will be provided at some level of the injection tree.
