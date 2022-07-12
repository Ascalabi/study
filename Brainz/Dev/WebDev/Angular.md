# Angular
Angular is a [[JavaScript]] **Framework** which lets you create [[Single Page Applications]] (SPA). It allows us to mix static HTML and dynamic things. Angular uses [[TypeScript]] which is like some [[JavaScript]] bullshit.

We basically have only one html file but it looks like we have more pages, JavaScript is much faster this way. JavaScript Changes the [[DOM]]

We don't have any loading. Kinda awesome ngl


## Installation
```bash
#installing angular
npm install -g @angular

# create a new workspace amd initial starter app
ng new my-app
# issues... had to change the package.json version of typescript to ^4.0.0 and run npm install

# launches the server
ng serve --open
```

```html
<input type="text" [(ngModel)]="name">
```
## Modules
**app.module.ts** for adding packages from angular
```javascript
import { FormsModule } from '@angular/forms';

# and add it to imports: []

```

> **imports**

## Component
**Component** is a bundle of functionality. Consists of a **component class** that handles data and functionality, and **HTML template** and **CSS** file.

**app-root** is the first component that gets loaded and the container for other components. So all other components are added to this one.

@Component() specifies a CSS selectorthat defines how the component is  used in a tamplate, HTML template and CSS styles.
```
@Component({
  selector: 'app-name', #the component's CSS element selector (basically a name called from html)
  templateUrl: './name.component.html',
  styleUrls: ['./name.component.css']
})
```

Every component has to have a **template**

### Adding one
Create a folder and put stuff in there... Create a ts and a html file

server.component.ts
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-server',
  templateUrl: './server.component.html'
});

export class ServerComponent {

}
```
And we need to add it to **app.module.ts** to declarations and import it... up

Add this to app.component.html
```html
<app-server></app-server>
```

#### Bruh fuck that samo uporabi ng generate component or ng g c 

## Databinding
Is the communication between the TypeScript code and the Template(HTML).

We write this shit in **export class  name**...

### String Interpolation
Kinda cool lahko delas tak shit
```html
<button (click)="switchLoginMode()" >Change shit {{ isLoginMode ? 'Register' : 'Already registered?'}}</button>
```

### Property binding
[property]="attribute" will tell angular that we want this property dynamic.

```html
<button [disabled]="!allowNewServer">Add</button>
```

### Event binding
ummmm
```html
<button [disabled]="!allowNewServer" (click)="onCreateServer()">Add</button>
```


## Features
### Dirtectives
Are instruction in the DOM. Declarative templates let you cleanly separate application's logic from its  presentation. Bruh kinda genius

```html
<h2>Hello World: ngIf!</h2>

<button (click)="onEditClick()">Make text editable!</button>

<div *ngIf="canEdit; else noEdit">
    <p>You can edit the following paragraph.</p>
</div>

<ng-template #noEdit>
    <p>The following paragraph is read only. Try clicking the button!</p>
</ng-template>

<p [contentEditable]="canEdit">{{ message }}</p>
```

### Dependecny injection
:(

### Pipes
Pipes are referred as filters. Check them out. We can create them with ng g pipe 

```html
<div>
  Today's date :- {{presentDate | date:"medium" | uppercase}}; 
</div>

```

## CLI

> **ng build**  compiles angular app into an output directory
> 
> **ng serve** Buildes and serves your app
> 
> **ng generate** 
>> **ng generate component** creates component, adds to the AppModule
> 
> **ng test** runs unit test



## Routing
Can create Routing with **ng new routing-app --routing**
```javascript
import { Routes, RouterModule } from '@angular/router';  //import this

//this are your routes
const appRoutes: Routes =[
  { path: '', component: HomeComponent },
  { path: 'auth', component: AuthComponent }
]

//add this to the imports
RouterModule.forRoot(appRoutes)
```

```js
{ path: '**', component: PageNotFoundComponent },  // Wildcard route for a 404 page
```


## Authentication
The process of recognizing the user's identity.

## Authorization 
The process of giving permission to the user to access certain resource in the system.

## Random important stuff

### Constructor and OnInit differences	
**Constructor** is a default method of the class that is executed when the class is instantiated and ensures proper initialisation of fields in the class and its subclasses. Angular, or better Dependency Injector (DI), analyses the constructor parameters and when it creates a new instance by calling new MyClass() it tries to find providers that match the types of the constructor parameters, resolves them and passes them to the constructor like

**ngOnInit** is a life cycle hook called by Angular to indicate that Angular is done creating the component. We have to import **OnInit**

**constructor** is called before 

Mostly we use ngOnInit for all the initialization/declaration and avoid stuff to work in the constructor. The constructor should only be used to initialize class members but shouldn't do actual "work".

