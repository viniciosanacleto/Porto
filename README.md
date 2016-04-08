# Freestyle Architecture

[![forthebadge](http://forthebadge.com/images/badges/built-by-developers.svg)](http://www.zalt.me)

<a name="Introduction"></a>
##Introduction


The Freestyle Architecture is a clean and easy to understand Software Architecture. 
It is inspired by the (DDD) Domain Driven Design Pattern and the (MVC) Model-View-Controller Architecture.

The Freestyle Architecture is designed to be Modular, Agile and Easy to understand. To help Developers build Scalable, Maintainable and Reusable Applications.




![](http://s9.postimg.org/4ay6fcm5r/betatesting.jpg)

##Content

- [Introduction](#Introduction)
	- [Quality Attributes](#Quality-Attributes)
	- [Terminology](#Terminology)
	- [Conventions](#Conventions)
	- [General Information](#General-Information)
- [How it works](#How-it-works)
- [Layers](#Layers)
	- [Interfaces](#Interfaces)
	- [Modules](#Modules)
	- [Infrastructure](#Infrastructure)
- [Layers Diagram](#Layers-Diagram)
- [Components](#Components)
	- [Routes](#Routes)
	- [Requests](#Requests)
	- [Controllers](#Controllers)
	- [Commands](#Commands)
	- [Models](#Models)
- [Folders-Structure](#Folders-Structure)
- [Development Workflow](#Development-Workflow)

<br>

>Business guys do not care about Code, you are the only one who care about it. So let the Code work for you!


![](http://s11.postimg.org/ay107nxlv/contemporary_paintings.jpg)



<a name="Quality-Attributes"></a>
###Quality Attributes

- Reusable Code (Modules of Business Logic).
- Simple files and folders structure.
- Fast Development (Easy to implement new features).
- Easy Maintenance (Easy to adapt changes).
- Super Scalable (Easy to modify and add features).
- Zero Technical debt (Low communication between Developers).
- Easy to Test.
- Easy Framework Upgrade (Decoupled application code from the framework code).
- Zero Code Decoupling.
- Easy to understand by any developer (less magic, more readable code).



<a name="Terminology"></a>
###Terminology

`Layer` is a logical term, used as indicate to the separation between things. In reality it's just a folder.

`Component` is a class, that has specific responsibility.

<a name="Conventions"></a>
###Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [[RFC2119](http://tools.ietf.org/html/rfc2119)].




<a name="General-Information"></a>
###General Information

The Freestyle Architecture is a way to structure the software by defining the relations between the application components of different layers.

The main goal is to organise the code base, by thinking of actions in isolation and using the code as modelling tool.

The Freestyle Architecture is very helpful for enterprise and long term projects, as these projects tends to have higher complexity with time.





<br>
<a name="How-it-works"></a>
##How it works

The Request life cycle:

1. **[Client Side]:** `User` calls a `Route` Endpoint (makes an HTTP `Request`)
2. **[Interface Layer]:** `Route` calls a `Controller`
3. **[Interface Layer]:** `Controller`read the `Request` input data
4. **[Interface Layer > Modules Layer]:** `Controller` call a `Command` and pass data to it
5. **[Modules Layer]:** `Command` performs some business logic *(call different components even `Commands` of the same `Module`)*
6. **[Modules Layer]:** `Command` returns *{something}* back to the `Controller`
7. **[Interface Layer]:** `Controller` builds a response and send it back to the `User`









<br>
<a name="Layers"></a>
##Layers

The Freestyle Architecture consist of 3 layers `Interfaces`, `Modules` and `Infrastructure`. *(Each layer contained in a folder)*.

Each layer contains some components *(components are classes like Models and Controllers)*. Some of those components are essential to a layer *(main components)* and some are optional *(additional components)*.


We'll discuss each one apart:
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

The main components of an **Interface** are `Routes`, `Requests` and `Controllers`.
<br>
The optional components can be `Response Transformers` and `Tests`.



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

The main components of a **Module** are `Commands` and `Models`. 
<br>
The optional components can be `Repositories`, `Exceptions`, `Service Providers`, `Database Migrations`, `Adapters`, `Factories`, `Facades`, `Services`, `Traits` and anything else...


<br>
<a name="Infrastructure"></a>
###Infrastructure

The **Infrastructure** layer is the glue between the application `Components` and the Framework.

The `Infrastructure` is what links everything with the framework (usually Infrastructure means the framework itself but in this case, it's the links to a framework).
<br>
The `Infrastructure` contains the reusable code between the `Modules` themselves and the `Interfaces` themselves. 
<br>
The `Infrastructure` separates the application code from the Framework code.
<br>
One of the major roles that the `Infrastructure` play, is facilitating the upgrading of the framework in the future without affecting a single line of the Application business logic.

The main components of the **Infrastructure** are `Abstracts`, `Service Providers`, `Traits` and `Exceptions`. 
<br>
The optional components can be `Database Criterias`, `Models Factories`.





<br>
<a name="Layers-Diagram"></a>
##Layers Diagram

![](http://s29.postimg.org/amlftwh13/freestyle_architecture.png)





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
<a name="Requests"></a>
###Requests

`Requests` are very useful to automatically apply the Validation rules. Once they are injected (in your `Controller`) they automatically check if the request data matches your validation rules, if not matched they throw an Exception immediately.
<br>
`Requests` class are very good place to write your validation rules. They can also be used to write your authorization code, to check if the user is authorized to make this request.

For more information about the `Requests` check [this](https://laravel.com/docs/requests).



<br>
<a name="Controllers"></a>
###Controllers

`Controllers` are the same as in MVC, but their responsibilities are limited.

`Controllers` act as the relation between the `Interfaces` layer and the `Modules` layer (by dispatching the `Modules` `Commands`).
<br>
The `Controller` has three main roles the first is reading the request data (user input), second dispatching `Commands` (and passing that data) and third building a Response from whatever the command has returned.
<br>
`Controllers` SHOULD not have any form of business logic. (It dispatches `Commands` to perform a business logic).
<br>
A `Controller` can dispatch multiple `Commands`.
<br>
Each `Controller` can only respond to a single `Route` (so every controller SHOULD have a single function).



<br>
<a name="Commands"></a>
###Commands

`Commands` are the Public API of the Module. 

The `Module` exposes a Command per Functionality (each functionality provided by a `Module` should be presented as `Command`).
<br>
The `Command` can do anything inside it's `Module` (even executing another `Commands` from the same Module).
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
___

<a name="Folders-Structure"></a>
##Folders Structure

Coming Soon.


<a name="Code-Sample"></a>
##Code Sample

Coming Soon. (Working in progress..)



## Help us Improve!
Let's help Developers write better code. For an easier Developers life :)



## Credits

- [Mahmoud Zalt](https://github.com/Mahmoudz)


## License
MIT









