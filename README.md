# Porto (Software Architectural Pattern)

## Welcome to Porto

- [Introduction](#Introduction)
- [Quality Attributes](#Quality-Attributes)
- [Layers](#layers)
	- [Layers Diagram](#Layer-Diagram)
	- [Ship Layer](#Ship-Layer)
	- [Containers Layer](#Containers-Layer)
		- [Structure](#Containers-Structure)
		- [Interactions](#Containers-Interactions)
		- [Example](#Containers-Example)
- [Components Categories](#Components-Categories)
	- [Main Components](#Main-Components)
		- [Components Interaction Diagram](#Components-Interaction-Diagram)
		- [Request Life Cycle](#Request-Life-Cycle)
	- [Optional Components](#Optional-Components)
- [Main Components Definitions & Principles](#Components-Details)
	- [Routes](#Routes)
	- [Controllers](#Controllers)
	- [Requests](#Requests)
	- [Actions](#Actions)
	- [Tasks](#Tasks)
	- [Models](#Models)
	- [Views](#Views)
	- [Transformers](#Transformers)
	- [Sub-Actions](#Sub-Actions)
- [Container Example](#Container-Example)
- [Projects](#Projects)
- [Feedback & Questions](#Feedback)
- [Credits](#Credits)
- [License](#License)



<a id="Introduction"></a>
# Introduction

**Porto SAP** is a modern Software Architectural Pattern, designed to help developers organize their Code in a highly maintainable way. Its main goal is to organize the business logic in a reusable way.

Porto is a great alternatives to the standard MVC, for large and long term projects, as they tend to have higher complexity over time.

Porto inherits concepts from the MVC, DDD, Modular and Layered architectures. And it adheres to a list of convenient design principles such as SOLID, OOP, LIFT, DRY, CoC, GRASP, Generalization, High Cohesion and Low Coupling.

> Porto started as an experimental architecture, trying to solve the common problems developers face, when building medium to large web projects. While all modular architectures focuses on the reusability of the framework general components, Porto focuses on the reusability of the business logic.



<br>

Feedbacks & Contributions are welcomed and credited. 

**Let's make the developer's lives easier.**


<a id="Quality-Attributes"></a>
# Quality Attributes

- Reusable Business Logic (Containers) across multiple similar projects.
- Easy to understand by any developer (No magic).
- Easy to maintain (easy to adapt changes).
- Easy to Test (test driven).
- Zero Technical debt (low communication between Developers).
- Decoupled Code (editing X doesn’t break Y).
- Pluggable UI, (start with Web App now and build an API later, or the opposite)
- Very Organized Code base with Zero Code Decoupling.
- Scalable Code (easy to modify and implement features).
- Easy Framework Upgrade (complete separation between the App and the Framework).
- Easy to locate any functionality.
- Organized coupling between the internal Components.
- Slim Classes (by following the Single Responsibility Principle).
- Uses the domain expert language when naming the Components.
- Clean and clear development workflow.




## Some of Porto's features in short details:


### Decoupling

Separation of concerns, is an important principle, Porto tries to follow.

In Porto the Appliction is separated from the framework that it lives in. 
And the business logic code is separated from infrastructure code, within the Appliction itself. 
As well as, the UI's (user interfaces) are separated from each others within the business logic code itself.

Making the UI's "`WEB`, `API`, `CLI`" pluggable, allows building the Web interface first and adding the API later or the opposite, with the least effort possible, since the `Actions` are the central organizing principle are they are shared accross UI's.


### Modularity

In Porto, your application business logic lives in `Containers` (same as Modules & Domains & Layers) and interact with other Containers in pre-defined directions.

In Porto, Containers are similar in concept to Modules *(from a Modular architectures)*. 
However, Containers in Porto do cross the bounderies of a Module, due to the fact that they contain business logic which by nature involves a lot of dependencies and communications. 

At the core, Models have relationships with Models from different Containers. And Actions can call Tasks and SubActions from other Containers. It's the responsibility of the developer to minimize the relationships between the Containers, while Porto's rules and guidlines will help achieving that.

In terms of dependancy management, the developer is free to move each Container to its own repository or keep all Containers together under single management. 

Porto benifits from both worlds it applies concepts from the Modular world in the simple Monolithic world.

### Testability

Writting testable code is high priority for Porto. Following the principles as advised will ensure you have easily testable code.

Porto includes Unit tests folders at the root of the Container. And also includes functional tests folders in each UI folder.


### Search-ability 

One of the biggest advantages of Porto, is the speed of finding a piece of code in a large code base.

Usually, developers build classes with set of related functions. And then start making calls between those functions of different classes.. The bigger the code gets the harder it becomes to find a specific function or track a feature down.

In porto the biggest class consist of single function and it's always having the same name `run`. 
This allows you to find any Use Case (`Action`) in your code by just browsing the files.

In Porto you can find any feature implementation in less than 3 seconds! (example: if you are looking for where the user address is being validated!, just go to the Address Container, open the list of Actions and search for ValidateUserAddressAction). 





<a id="layers"></a>
# Layers

At its core Porto consists of 2 layers (`Containers` & `Ship`) and set of `Components` with predefined responsibilities.

These Layers (folders) can be created anywhere inside your framework of choice.

*(example: in Laravel PHP you can place them in the `app/` directory or create an `src/` directory on the root)*


The high-level code (Application Business Logic), should be written in the Container Layer. While the low-level code will live in the Ship Layer. 

The Containers (high-level code) relies *indirectly* on the Ship (low-level code) to function. But not the opposite.


<a id="Layers-Diagram"></a>
## Layers Diagram


![](https://s19.postimg.org/cgh6yqtn7/porto_layers.png)

#### Visual Image:

![](https://s19.postimg.org/d79x4iw0j/porto_visual_diagram.png)





<br>

<a id="Ship-Layer"></a>

## Ship Layer:

The Ship layer, contains the engine of the Ship which autoloads all the Components of your Containers. 
It can also contain code that can be used by all your Containers Components.

The Ship layer, plays an important role in separating the Application code from the Framework code. 
Thus it facilitates upgrading the Framework without affecting the Application code.

In Porto the Ship layer is very slim, it doesn't contain common reusable functionalities such as Authentication or Authorization, all these functionalities live in their own separated Containers.


### Ship Structure

The Ship, contains:

- **Engine**: is the engine that auto-register all your Container's Components and boots your Application.
- **Parents**: contains the classes for each Component in your Container. (Adding functions to the parent classes makes them available in every Container).
- **Other Folders**: each of the other folders provide reusable features to be used by all Containers. Example, Global Exceptions, Application Middleware's, Global Config files...

Note: All the Container's Components MUST extend or inherit from the Ship layer *(in particular the Parents folder)*.




<br>

<a id="Containers-Layer"></a>

## Containers Layer:

The Containers layer is where the Application specific business logic is written *(Application functionalities)*.

Containers are wrappers of business logic.

Here's an example of how to decide creating Containers.
*"In a TODO App the Task, User and Calendar... each would be in it's own Container, were each has its own Routes, Controllers, Models, Exceptions, ect.. And each Container is responsible of receiving requests and returning responses from whichever UI (Web, API..) it support."*

It's advised to use Single Model per Container, however in some cases you might need more.
Just keep in mind two Models means two Repositories, two Transformers, etc...
Unless you want to use both Models always together, do split them into 2 Containers.





<a id="Containers-Structure"></a>
### Containers Structure:

```
Container 1
	├── Actions
	├── Tasks
	├── Models
	└── UI
	    ├── WEB
	    │   ├── Routes
	    │   ├── Controllers
	    │   └── Views
	    ├── API
	    │   ├── Routes
	    │   ├── Controllers
	    │   └── Transformers
	    └── CLI
	        ├── Routes
	        └── Commands

Container 2
	├── Actions
	├── Tasks
	├── Models
	└── UI
	    ├── WEB
	    │   ├── Routes
	    │   ├── Controllers
	    │   └── Views
	    ├── API
	    │   ├── Routes
	    │   ├── Controllers
	    │   └── Transformers
	    └── CLI
	        ├── Routes
	        └── Commands
```


<a id="Containers-Interactions"></a>
### Interactions between Containers
- A Container MAY depends on one or many other Containers.
- A Controller MAY run Tasks from another Container.
- A Model MAY have a relationship with a Model from other Containers.


<br>
<a id="Components-Categories"></a>

# Components Categories


Every Container consist of a number of Components, in **Porto** the Components are splitted into two categories:
`Main Components` and
`Optional Components`.



<a id="Main-Components"></a>
## Main Components:

You must use these Components as they are essential for almost all types of Web Apps:

Routes - Controllers - Requests - Actions - Tasks - Models - Views - Transformers.

> **Views:** should be used in case the App serves HTML pages.
> <br>
> **Transformers:** should be used in case the App serves JSON or XML data.


<a id="Components-Interaction-Diagram"></a>
### Main Components Interaction Diagram


![](https://s19.postimg.org/67vv55w2b/porto_container.png)


<a id="Request-Life-Cycle"></a>
### Request Life Cycle

*A typical very basic API call scenario:*

1. **User** calls an `Endpoint` in a `Route` file.
2. `Endpoint` calls a `Middleware` to handle the Authentication.
3. `Endpoint` calls its `Controller` function.
4. `Request` injected in the `Controller` automatically applies the request validation & authorization rules.
5. `Controller` calls an `Action` and pass each `Request` data to it.
6. `Action` calls multiple `Tasks` to perform the business logic, *{or it handles all the job itself}*.
7. `Tasks` performs the business logic (every `Task` does a single portion of the main Action).
8. `Action` collects data from the `Tasks` and returns them to the `Controller`.
9. `Controller` builds the response using a (`View` or `Transformer`) and send it back to the **User**.





<a id="Optional-Components"></a>
## Optional Components:

You can add these Components when you need them, based on your App needs, however some of them are highly recommended:

Repositories - Exceptions - Criterias - Policies - Tests - Middlewares - Service Providers - Events - Commands - DB Migrations - DB Seeders - Data Factories - Contracts - Traits - Jobs...



<br>


<a id="Components-Details"></a>
# Main Components Definitions & Principles

Below is the explanation of the roles, usage and principles of each Main Component.




<a id="Routes"></a>
## Routes
 
Routes are the first receivers of the HTTP requests.

The Routes are responsible for mapping all the incoming HTTP requests to their controller's functions.

The Routes files contain Endpoints (URL patterns that identify the incoming request).

When an HTTP request hits your Application, the Endpoints match with the URL pattern and make the call to the corresponding Controller function.

#### Principles:
- There are three types of Routes, API Routes, Web Routes and CLI Routes.
- The API Routes files SHOULD be separated from the Web Routes files, each in its own folder.
- The Web Routes folder will contain only the Web Endpoints, (accessible by Web browsers); And the API Routes folder will contain only the API Endpoints, (accessible by any consumer App).
- Every Container SHOULD have its own Routes.
- Every Route file SHOULD contain a single Endpoints.
- The Endpoint job is to call a function on the corresponding Controller once a request of any type is made. (It SHOULD NOT do anything else).


<a id="Controllers"></a>
## Controllers

Controllers are responsible for serving the request data and building responses.

The Controllers concept is the same as in MVC *(They are the C in MVC)*, but with limited and predefined responsibilities.

#### Principles:
- Controllers SHOULD NOT know anything about the business logic or about any business object.
- A Controller SHOULD only do the following jobs:
   1. Reading a Request data (user input)
   2. Calling an Action (and passing request data to it)
   3. Building a Response (usually build response based on the data collected from the Action call)
- Controllers SHOULD NOT have any form of business logic. (It SHOULD call an Action to perform the business logic).
- Controllers SHOULD NOT call Container Tasks. They MAY only call Actions. (And then Actions can call Container Tasks).
- Controllers CAN be called by Routes Endpoints only.
- Every Container UI folder (Web, API, CLI) will have its own Controllers.



<a id="Requests"></a>
## Requests

Requests mainly serves the user input in the application. And they are very useful to automatically apply the Validation and Authorization rules.

Requests are the best place to apply validations, since the validations rules will be related to every request. 
Requests can also check of Authorization to check if this user has access to this controller function. 
*(Example: check if this user own that product before deleting it, or check if this user is admin to do edit something).*

#### Principles:
- A Request MAY hold the Validation / Authorization rules.
- Requests SHOULD only be injected in Controllers. Once injected they automatically check if the request data matches the validation rules, and if the request input is not valid an Exception will be thrown.
- Requests MAY also be used for authorization, they can check if the user is authorized to make a request.



<a id="Actions"></a>
## Actions

Actions represent the Use Cases of the Application *(the actions that can be taken by a User or a Software in the Application)*. 

Actions CAN hold business logic or/and they orchestrate the Tasks to perform the business logic.

Actions take data structures as inputs, manipulates them according to the business rules internally or through some Tasks, then output a new data structures.

Actions SHOULD NOT care how the Data is gathered, or how it will be represented.

By just looking at the Actions folder of a Container, you can determine what Use Cases (features) your Container provides. 
And by looking at all the Actions you can tell what an Application can do.



#### Principles:
- Every Action SHOULD be responsible for doing a single Use Case in the Application.
- An Action MAY retrieves data from Tasks and pass data to another Task.
- An Action MAY call multiple Tasks. (They can even call Tasks from other Containers as well!).
- Actions MAY return data to the Controller.
- Actions SHOULD NOT return a response. (the Controller job is to return a response).
- An Action CAN call another Action (but it's recommended not to do that).
- Actions are mainly used from Controllers. However, they can be used from Events, Commands and/or other Classes. But they SHOULD NOT be used from Tasks.
- Every Action SHOULD have only a single function named `run()`.




<a id="Tasks"></a>
## Tasks

The Tasks are the classes that holds shared business logic between multiple Actions. 

Every Task is responsible for little part of the logic.

Tasks are optional, but in most cases you find yourself in need for them.

Example: Let's say we have an Action 1 that needs to find a record by its ID from the DB, then fires an Event and pass the record data to it. 
And we have an Action 2 that needs to find the same record by its ID then makes a call to an API and pass the record data to it.
Since both actions are performing the "find a record by ID" job, it would be much better to have a task the does this job and returns that record.

Whenever you see the possibility of reusing a piece of code from an Action, you should put that piece of code in a Task.




#### Principles:
- Every Task SHOULD have a single responsibility (job).
- An Action MAY receive and return Data. (Actions SHOULD NOT return a response, the Controller job is to return a response).
- A Task SHOULD NOT call another Task. Because that will takes us back to the Services Architecture and it's a big mess.
- A Task SHOULD NOT call an Action. Because your code wouldn't make any logical sense then!
- Tasks SHOULD only be called from Actions. (They could be called from Actions of other Containers as well!).
- Tasks usually need a single function `run`. However, if you prefer you can have more than one function, but make sure to explicitly name them "example: `FindUserTask` can have 2 functions `byId` and `byEmail`". 
- A Task SHOULD NOT be called from Controller. Because this leads to non-documented feature in your code. It's totally fine to have a lot of Actions "example: `FindUserByIdAction` and `FindUserByEmailAction` where both Actions are calling the same Task" as well as it's totally fine to have single Action `FindUserAction` making a decision to which Task it should call.


<a id="Models"></a>
## Models

The Models provide an abstraction for the data, they represent the data in the database. *(They are the M in MVC)*.

Models are responsible for how the data should be handled. They make sure that data arrives properly into the backend store (e.g. Database).

#### Principles:
- A Model SHOULD NOT hold business logic, it can only hold the code and data the represents itself. *(it's relationships with other models, hidden fields, table name, fillable attributes,...)*
- A single Container MAY contains multiple Models.
- Model MAY define the Relations between itself and any other Models (in case a relation exist).



<a id="Views"></a>
## Views

Views contain the HTML served by your application. 

Their main goal is to separate the application logic from the presentation logic. *(They are the V in MVC)*.

#### Principles:
- Views can only be used from the Web Controllers.
- View SHOULD be separated into multiple files and folders based on what they display.
- A single Container MAY contains multiple Views files.


<a id="Transformers"></a>
## Transformers

Transformers (are the short name for Responses Transformers). 

They are the same like Views but for JSON Responses. While Views takes data and represent it in HTML, Transformers takes data and represent it in JSON.

Transformers are classes responsible for transforming Models into an Arrays.

Transformers takes a Model or a group of Models "Collection" and converts it to a formatted serializable Array.


#### Principles:
- All API responses MUST be formatted via a Transformer.
- Every Model (that gets returned by an API call) SHOULD have a Transformer.
- A single Container MAY have multiple Transformers.
- Usually every Model would have a Transformer.




<a id="Sub-Actions"></a>
## Sub-Actions

SubActions are designed eliminate code duplication in Actions. Don't get confused! SubActions do not replace Tasks.

While Tasks allows Actions to share a piece of functionality. SubActions allows Actions to share a seequence of Tasks.

*Example: If an Action is calling Task1, Task2 and Task3. And another Action is calling Task2, Task3 and Task4. (And both Actions are throwing the same Exception when Task2 returns `Foo`). Would be very useful to extract that code into a SubAction, and make it reusable.*



#### Principles:
- Sub-Actions MUST call Tasks. If a Sub-Actions is doing all the business logic, without the help of at least 1 Tasks (recommended 2 minimum), It probably shouldn't be a Sub-Action.
- A Sub-Action MAY retrieves data from Tasks and pass data to another Task.
- A Sub-Action MAY call multiple Tasks. (They can even call Tasks from other Containers as well!).
- Sub-Actions MAY return data to the Action.
- Sub-Action SHOULD NOT return a response. (the Controller job is to return a response).
- Sub-Action SHOULD NOT call another Sub-Action. (try to avoid that as much as possible).
- Sub-Action SHOULD be used from Actions. However, they can be used from Events, Commands and/or other Classes. But they SHOULD NOT be used from Controllers or Tasks.
- Every Sub-Action SHOULD have only a single function named `run()`.



<br>





<a id="Container-Example"></a>
## Typical Container Example:

```
Container
	├── Actions
	├── Tasks
	├── Models
	├── Events
	├── Policies	
	├── Exceptions
	├── Contracts
	├── Traits
	├── Jobs
	├── Notifications
	├── Providers
	├── Configs
	├── Data
	│   ├── Migrations
	│   ├── Seeders
	│   ├── Factories
	│   ├── Criterias
	│   └── Repositories
	├── Tests
	│   ├── Unit
	│   └── Traits
	└── UI
	    ├── API
	    │   ├── Routes
	    │   ├── Controllers
	    │   ├── Requests
	    │   ├── Transformers
	    │   └── Tests
	    │       └── Functional
	    ├── WEB
	    │   ├── Routes
	    │   ├── Controllers
	    │   ├── Requests
	    │   ├── Views
	    │   └── Tests
	    │       └── Acceptance
	    └── CLI
	        ├── Routes
	        ├── Commands
	        └── Tests
	            └── Functional
```






<br>

<a id="Projects"></a>

# Projects


> Ready to see the implementions!

- **PHP**
	- [**apiato**](http://apiato.io/) *(A flawless framework for building scalable and testable API-Centric Apps in PHP.)*
- **Python**
- **Ruby**
- **Java**
- **C#**




<a id="Feedback"></a>
# Get in Touch

**Your feedback helps.**

For feedbacks, questions or suggestions? We are on [**Slack**](https://now-examples-slackin-bvfqosqozk.now.sh/).

[![](https://s19.postimg.org/h7pvzy9ar/Slack-i_OS-icon.png)](https://now-examples-slackin-bvfqosqozk.now.sh)



<a id="Credits"></a>
## Credits


| Authors      | Twitter                                           | Site                       | Email           |
|--------------|---------------------------------------------------|----------------------------|-----------------|
| Mahmoud Zalt | [@Mahmoud_Zalt](https://twitter.com/Mahmoud_Zalt) | [zalt.me](https://zalt.me) | mahmoud@zalt.me |


<a id="Donations"></a>
## Donations

[![Beerpay](https://beerpay.io/apiato/apiato/badge.svg?style=beer-square)](https://beerpay.io/apiato/apiato)  
[![Beerpay](https://beerpay.io/apiato/apiato/make-wish.svg?style=flat-square)](https://beerpay.io/apiato/apiato?focus=wish)


<a name="License"></a>
## License

The MIT License [(MIT)](https://opensource.org/licenses/MIT).
