# Freestyle Architecture

[![forthebadge](http://forthebadge.com/images/badges/built-by-developers.svg)](http://www.zalt.me)


The Freestyle Architecture is a clean and easy to understand Software Architecture. 
It is inspired by the (DDD) Domain Driven Design Pattern and the (MVC) Model-View-Controller Architecture.

The Freestyle Architecture is designed to be Modular, Agile and Easy to understand. To help Developers build Scalable, Maintainable and Reusable Web Applications.


![](http://s9.postimg.org/4ay6fcm5r/betatesting.jpg)


**Quality Attributes: (Advantages)**

- Reusable Code (Modules of Business Logic).
- Fast Development.
- Easy Maintenance.
- Super Scalable (Easy to modify and add features).
- Zero Technical debt (Low communication between Developers).
- Easy to Test.
- Easy Framework Upgrade (Decoupled application code from the framework code).
- Zero Code Decoupling.
- Easy to understand by any developer (less magic, more readable code).

<br>

>Business guys do not care about Code, you are the only one who care about it. So let the Code work for you, don't work for it!


![](http://s11.postimg.org/ay107nxlv/contemporary_paintings.jpg)


##Content

<MEGA-PRO-----------------------------------------------------------MEGA-PRO>




- [Introduction](#Introduction)
- [Layers](#Layers)
	- [Interfaces](#Interfaces)
	- [Modules](#Modules)
	- [Infrastructure](#Infrastructure)
- [Layers Diagram](#Layers-Diagram)
- [How it works](#How-it-works)
- [Components](#Components)
	- [Routes](#Routes)
	- [Controllers](#Controllers)
	- [Commands](#Commands)
	- [Models](#Models)
	- [Transformers](#Transformers)
	- [Requests](#Requests)
	- [Repositories](#Repositories)
	- [Criterias](#Criterias)
	- [Exceptions](#Exceptions)
	- [Tests](#Tests)
	- [Providers](#Providers)
- [Folders-Structure](#Folders-Structure)
- [Development Workflow](#Development-Workflow)



<a name="Introduction"></a>
##Introduction

The Freestyle Architecture consist of 3 layers `Interfaces`, `Modules` and `Infrastructure`. *(Each layer contained in a folder)*.

Each layer contains some components *(components are classes like Models and Controllers)*. Some of those components are essential to a layer *(main components)* and some are optional *(additional components)*.


<br>
<a name="Layers"></a>
##Layers

The architecture consist of 3 layers we'll discuss each one apart:
<br>
And we'll explain the usage of each layer and show you why it's important.


<br>
<a name="Interfaces"></a>
###Interfaces

The **Interfaces** layer contains the public API's of the application.

An `Interface` is what the user uses to access an application. 
<br>
It can be an `API` (responding to Endpoints calls), `CLI` (executing Commands) or anything else you prefer.

However in the Freestyle Architecture we try our best to implement a modern coding practices. 
Thus we recommend not making a `WEB` Interface that serves HTML Pages, instead build your `WEB` App with a JS Framework like (AngularJS or ReactJS or...) and let your App request Data from your `API`.
This way you don't have to change your code to support new devides in the future like (Mobile App, Tablets App or Desktop App) since all your Apps can use the same `API`. 
And you can even allow other Apps to integrate with your App real quick, since you already have your `API` ready.

The main components of an `Interface` are `Routes` and `Controllers`.
<br>
The optional components can be `Requests`, `Transformers` and `Tests`.



<br>
<a name="Modules"></a>
###Modules

The **Modules** layer contains the Application specific business logic. *(Application functionalities)*.

Each analogical business logic SHOULD be encapsulated insde a separate `Module`. *(All similar functionalities should be wrapped in a Module)*.
<br>
All the `Module` functionalities MUST be exposed via `Commands` and ONLY accessible via their `Commands`. *(We'll see what a Command is below)*.
<br>
Every `Module` MUST contain at least one `Command`. *(A Module can have one or many Commands)*.
<br>
`Modules` should not interact with each others directly, (the `Controllers` will control the interaction between `Modules`).
<br>
`Modules` can be reused across different projects. *(Think of them as third-party packages)*.

The main components of a `Module` are `Commands` and `Models`. 
<br>
The optional components can be `Repositories`, `Services`, `Exceptions`, `Providers`, `Migrations`, `Criterias`, `Adapters`, `Factories`, `Facades` and anything else...


<br>
<a name="Infrastructure"></a>
###Infrastructure

The **Infrastructure** layer is the glue between the application `Components` and the Framework. *(In this example it's Laravel)*.

The `Infrastructure` is what links everything with the framework (usually Infrastructure means the framework itself but in this case, it's the links to a framework).
<br>
The `Infrastructure` contains the reusable code between the `Modules` themselves and the `Interfaces` themselves. 
<br>
The `Infrastructure` separates the application code from the Framework code.
<br>
Usually, you don't have to touch this layer code. (Unless you know what you are doing).

One of the major roles that the `Infrastructure` play, is facilitating the upgrading of the framework in the future without affecting a single line of the Application business logic.



<br>
<a name="Layers-Diagram"></a>
##Layers Diagram

![](http://s29.postimg.org/amlftwh13/freestyle_architecture.png)




<br>
<a name="How-it-works"></a>
##How it works

The Request life cycle:

1. **[Client Side]:** `User` calls a `Route` Endpoint (makes an HTTP `Request`)
2. **[Interface Layer]:** `Route` calls a `Controller`
3. **[Interface Layer > Modules Layer]:** `Controller` dispatches a `Command`
4. **[Modules Layer]:** `Command` performs some business logic and returns *{something}* to the `Controller`
5. **[Interface Layer]:** `Controller` builds a response and send it back to the `User`






<br>
<a name="Components"></a>
##Components

Each layer in the architecture consist of multiple components, we'll discuss each one apart. 
<br>
And we'll explain the role of each component and show how it can be used.

<br>
<a name="Routes"></a>
###Routes

The `Routes` are the first receivers (ports) of the Request.

Not all `Interface` need to have `Routes` like the **CLI** `Interface`.
<br>
However the **API** `Interface` is one of the Interfaces that requires to have `Routes` inside it.
<br>
The `Routes` `(A.K.A. Endpoints)` can be placed inside multiple routes files each representing a specific version like `(api.v1, api.v2)`
<br>
The `Route` main job is to call a particular `Controller` once a Request is made.
<br>
Each `Route` can represent a single API Endpoint.
<br>
Each `Route` SHOULD have a dedicated `Controller` for it. 
<br>
A `Route` SHOULD only call the `handle` Function on it's `Controller`.

For more information about the `Routes` read [this](https://laravel.com/docs/routing/).


<br>
<a name="Controllers"></a>
###Controllers

`Controllers` are the same as in MVC, the only difference here is that a `Controller` can only respond to a single `Route`.

`Controllers` act as the relation between the `Interfaces` layer and the `Modules` layer (by dispatching the `Modules` `Commands`).
<br>
The `Controller` has two main roles, first dispatching `Commands` and second building a Response.
<br>
`Controllers` SHOULD not have any form of business logic. (It dispatches `Commands` to perform a business logic).
<br>
A `Controller` can dispatch multiple `Commands`.
<br>
Each `Controller` Class is ONLY responsible for one `Route`.



<br>
<a name="Commands"></a>
###Commands

`Commands` are the Public API of the Module. 

The `Module` exposes a Command per Functionality (each functionality provided by a `Module` should be presented as `Command`).
<br>
The `Command` can do anything inside it's `Module` (except executing `Commands`).
<br>
A `Command` can return `Data`, but SHOULD not return a response (the Controller job is to return a response).
<br>
The reusable code of the `Module` (reusable between different `Commands` in the same `Module`) can be extracted into `Services`.



<br>
<a name="Models"></a>
###Models

`Models` are objects representing data, in the database. (same as in MVC Architecture).

Each `Model` SHOULD define the Relations between itself and any other `Model` (in case a relation exist).






<br>
<a name="Transformers"></a>
###Transformers

`Transformers` are responsible for taking an instance of a `Model` and converting it to a formatted Array that is easy to be *Serialized*.

For more information about the `Transformers` read [this](http://fractal.thephpleague.com/transformers/).





<br>
<a name="Requests"></a>
###Requests

`Requests` are very useful to automatically apply the Validation rules. Once they are injected (in your `Controller`) they automatically check if the request data matches your validation rules, if not matched they throw an Exception immediately.
<br>
`Requests` class are very good place to write your validation rules. They can also be used to write your authorization code, to check if the user is authorized to make this request.

For more information about the `Requests` check [this](https://laravel.com/docs/requests).







<br>
<a name="Repositories"></a>
###Repositories

`Repositories` are the implementation of the Repository Design Pattern.

`Repositories` save and retrieve `Models` to or from the underlying storage mechanism.
<br>
The `Repository` is used to separate the logic that retrieves the data and maps it to a `Model`, from the business logic that acts on the `Model`.
<br>
Every `Model` SHOULD have a `Repository`. 
<br>
You MUST not directly access a `Model` to perform any query. Instead, you SHOULD do this through the Repository.



<br>
<a name="Criterias"></a>
###Criterias

`Criterias` are a way to change the `Repository` of the query by applying specific conditions according to your needs.

For more information about the `Criteria` read [this](https://github.com/andersao/l5-repository#create-a-criteria).





<br>
<a name="Exceptions"></a>
###Exceptions

`Exceptions` are classes to be thrown in case of errors.
<br>
`Exceptions` can be thrown from anywhere in the application.


<br>
<a name="Tests"></a>
###Tests

The two most essential `Tests` types in this architecture are the Unit Tests and the Functional Tests. (Additional `Tests` types are Integration Tests and Acceptance Tests).

`Interface` should be covered by Functional `Tests`, (Example testing the `Routes` are doing what's expected from them).
<br>
`Module` should be covered by Unit `Tests`, (Example testing the `Command` it's doing it's job).




<br>
<a name="Providers"></a>
###Service Providers

`Service Providers` let the Layers register some *stuff* into the framework service container.

Each `Module` can have it's own `Service Providers` *(one `Service Provider` per `Module` is perfect)*.
<br>
The `Infrastructure` layer SHOULD register the `Service Providers` of all the `Modules`. And then the `Infrastructure` Master `Service Providers` SHOULD be registered in the Framework.

For more information about the `Service Providers` read [this](https://laravel.com/docs/master/providers).















<br>
___

<a name="Folders-Structure"></a>
##Folders Structure

Comming Soon..



<a name="Code-Sample"></a>
##Code Sample

Coming Soon..



## Help us improve!
Let's help Developers write better code. For an easier Developers life :)



## Credits

- [Mahmoud Zalt](https://github.com/Mahmoudz)


## License
MIT









