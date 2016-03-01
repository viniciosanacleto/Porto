# Freestyle Architecture

[![forthebadge](http://forthebadge.com/images/badges/built-by-developers.svg)](http://www.zalt.me)

The Freestyle Architecture is a clean and easy to understand Software Architecture. 
It is inspired by the (DDD) Domain Driven Design Pattern and the (MVC) Model-View-Controller Architecture.

The Freestyle Architecture was designed to be Modular, Agile and Easy to understand. To help Developers build Scalable, Maintainable and Reusable Web Applications.

**Quality Attributes: (Advantages)**

- Reusable Code (Modules of Business Logic).
- Fast Development.
- Easy Maintenance.
- Super Scalable (Easy to modify and add features).
- Zero Technical debt (Low communication between Developers).
- Easy to Test.
- Easy Framework Upgrade (Decoupled application code from the framework code).
- Zero Code Decoupling.

>Business guys do not care about Code, you are the only one who care about it. So let the Code work for you, don't work for it!

##Content

- [Introduction](#Introduction)
	- [Interfaces](#Interfaces)
	- [Modules](#Modules)
	- [Infrastructure](#Infrastructure)
- [How it works](#How-it-works)
	- [Layers and Components Diagram](#Layers-and-Components-Diagram)
- [Components](#Components)
	- [Routes](#Routes)
	- [Controllers](#Controllers)
	- [Commands](#Commands)
	- [Models](#Models)
	- [Transformers](#Transformers)
	- [Inputs](#Inputs)
	- [Repositories](#Repositories)
	- [Criterias](#Criterias)
	- [Exceptions](#Exceptions)
	- [Tests](#Tests)






<a name="Introduction"></a>
##Introduction

The Freestyle Architecture is easy to understand and to maintain Software Architecture. 
It consist of 3 layers `Interfaces`, `Modules` and `Infrastructure`. Each layer contained in a folder.



<br/>
<a name="Interfaces"></a>
###Interfaces

The **Interfaces** layer contains the public API's of the application.

An `Interface` is what the user uses to access an application. 
<br/>
It can be an `API` (responding to Endpoints calls), `CLI` (executing Commands) or anything else you prefer.

However in the Freestyle Architecture we try our best to implement the latest modern coding practices. 
Thus we recommend not making a `WEB` Interface that serves HTML Pages, instead build your `WEB` App with a JS Frameowrk like (AngularJS or ReactJS or...) and let your App request Data from your `API`.
This way you don't have to change your code to support new devides in the future like (Mobile App, Tablets App or Desktop App) since all your Apps can use the same `API`. 
And you can even allow other Apps to integrate with your App real quick, since you already have your `API` ready.

The main components in Interfaces layer are `Routes` and `Controllers`.
<br/>
Optional components are `Transformers` and `Tests`.



<br/>
<a name="Modules"></a>
###Modules

The **Modules** layer contains the application specific business logic. 

Each `Module` encapsulates a part of the application logic.
<br/>
The `Module` functionality is exposed via `Commands` and it's ONLY accessible via `Commands`.
<br/>
Every `Module` MUST contain at least one `Command`.
<br/>
`Modules` should not interact with each others directly, (the `Controllers` will control the interaction between the `Modules`).
<br/>
`Modules` can be reused across different projects.

The main components in Modules layer are `Commands` and `Models`. 
<br/>
Optional components are `Inputs`, `Exceptions`, `Repositories` and `Criterias`.

In the `Modules` layer, you are free to add as many more components as you prefer: `Services`, `Adapters`, `Factories`, `ServiceProviders`, `Facades`,...


<br/>
<a name="Infrastructure"></a>
###Infrastructure

The **Infrastructure** layer is the glue between the `Components` and the Framework. 

The `Infrastructure` is what links everything with the framework (usually Infrastructure means the framework itself but in this case, it's the links to the framework).
<br/>
The `Infrastructure` contains the reusable code between the `Modules` themselves and the `Interfaces` themselves. 
<br/>
The `Infrastructure` separates the application code from the Framework code. (to easily upgrade the framework).
<br/>
Usually, no need to touch this layer (folder).








<br/>
<a name="How-it-works"></a>
##How it works

The Request life cycle:

1. the `User` makes a `Request`
2. the `Route`, calls a `Controller` (on the `Interface` layer)
3. the `Controller` dispatches a `Command`  (on the `Module` layer)
4. the `Command` performs the business logic (inside the `Module` itself) and returns {anything} back to the `Controller` (on the `Interface` layer)
5. the `Controller` return {anything} coming from the `Command` back to the `User`





<br/>
<a name="Layers-and-Components-Diagram"></a>
###Layers and Components Diagram

![](http://s28.postimg.org/hv3ercpwt/freestyle_architecture.png)



###Folders Structure

![](http://s24.postimg.org/rmy7xyhdx/freestyle_architecture_folders_structure.png)






<br/>
<a name="Components"></a>
##Components

Each `Layer` in the Freestyle Architecture consist of multiple `Components`.


<br/>
<a name="Routes"></a>
###Routes

The `Routes` are the first receivers (ports) of the Request. (Same as Laravel's Routes)

The `Route` main job is to call a particular `Controller` once a Request is made.
<br/>
Each `Route` represent a single API Endpoint.
<br/>
Each `Route` SHOULD have a dedicated `Controller` for it. 
<br/>
A `Route` SHOULD only call the `handle` Function on it's `Controller`.
<br/>
All the API `Routes` SHOULD be in the `src/backEnd/Interfaces/Api/Routes` folder.
<br/>
Each Routes file represent a version of your API (example the default file is `Api.v1.php`) to add a new version edit the `map` function in `src/backEnd/Infrastructure/Providers/ApiRouteServiceProvider.php`.


`Routes` code sample:

Normal Endpoint:

```php
$router->post('login', [
    'uses' => 'LoginController@handle'
]);
```

Protected Endpoint:
(User must login first before accessing this endpoint.)

```php
$router->get('users', [
    'uses'       => 'GetAllUsersController@handle',
    'middleware' => ['api.auth']
]);
```




<br/>
<a name="Controllers"></a>
###Controllers

`Controllers` are the same as in MVC, the only difference here is that a `Controller` can only respond to a single `Route`.

`Controllers` act as the relation between the `Interfaces` layer and the `Modules` layer (by dispatching the `Modules` `Commands`).
<br/>
The `Controller` has two main roles, first dispatching `Commands` and second building a Response.
<br/>
`Controllers` SHOULD not have any form of business logic. (It dispatches `Commands` to perform a business logic).
<br/>
A `Controller` can dispatch multiple `Commands`.
<br/>
Each `Controller` Class is ONLY responsible for one `Route`.
<br/>
Each `Controller` MUST have a `handle` function inside it.
<br/>
All the API `Controllers` SHOULD be in the `src/backEnd/Interfaces/Api/Controllers` folder.


`Controller` code sample:

```php
namespace Mega\Interfaces\Api\Controllers;

use Mega\Infrastructure\Abstracts\ApiController;
use Mega\Interfaces\Api\Transformers\UserTransformer;
use Mega\Modules\Authentication\Commands\LoginCommand;

class LoginController extends ApiController
{
    public function handle()
    {
        // 1. dispatching the Login Command on the Authentication Module
        $user = $this->dispatch(LoginCommand::class);

		 // 2. build a response with a Transformer
        return $this->response->item($user, new UserTransformer());
    }
}

```




<br/>
<a name="Commands"></a>
###Commands

`Commands` are the Public API of the Module. 

The `Module` exposes a Command per Functionality (each functionality provided by a `Module` should be presented as `Command`).
<br/>
The `Command` can do anything inside it's `Module` (except executing `Commands`).
<br/>
A `Command` can return `Data`, but SHOULD not return a response (the Controller job is to return a response).
<br/>
The reusable code of the `Module` (reusable between different `Commands` in the same `Module`) can be extracted into `Services`.


`Command` code sample:

```php
namespace Mega\Modules\Authentication\Commands;

use Mega\Infrastructure\Abstracts\Command;
use Mega\Modules\Authentication\Inputs\LoginInput;
use Mega\Modules\Authentication\Services\AuthenticationService;

class LoginCommand extends Command
{
    public function handle(LoginInput $loginInput, AuthenticationService $authenticationService)
    {
        $token = $authenticationService->authenticate($loginInput);
        $user = $authenticationService->getAuthenticatedUser();
        $userWithToken = $authenticationService->injectToken($token, $user);

        return $userWithToken;
    }
}
```

**Command usage:**
`Commands` can be dispatched from `Controllers`

```php
$user = $this->dispatch(LoginCommand::class);
```





<br/>
<a name="Models"></a>
###Models

`Models` are objects representing data, in the database. (same as in MVC Architecture).

Each `Model` SHOULD define the Relations between itself and any other `Model` (in case a relation exist).


`Model` code sample:

```php
namespace Mega\Modules\User\Models;

use Mega\Infrastructure\Abstracts\Model;
use Mega\Modules\User\Models\Account;


class User extends Model
{
	 // the database table used by this model
    protected $table = 'users';
    
    // defining the relation between the User and the Account Models
    public function account()
    {
        return $this->hasOne(Account::class);
    }
}
```




<br/>
<a name="Transformers"></a>
###Transformers

`Transformers` are responsible for taking an instance of a `Model` and converting it to a formated Array that is easy to be *Serialized*.

Each `Transformer` MUST have a `transform` function
<br/>
All the API `Transformers` SHOULD be in the `src/backEnd/Interfaces/Api/Transformers` folder.
<br/>
For more information about the `Transformers` concept, read [this](http://fractal.thephpleague.com/transformers/).



`Transformer` code sample:

```php
namespace Mega\Interfaces\Api\Transformers;

use Mega\Infrastructure\Abstracts\Transformer;
use Mega\Modules\User\Models\User;

class UserTransformer extends Transformer
{
    public function transform(User $user)
    {
        return [
            'id'    => (int)$user->id,
            'name'  => $user->name,
            'email' => $user->email,
            'token' => $user->token,
        ];
    }
}
```
**Input usage:**

```php
// getting any Model
$user = $this->getUser();
// building the response with the transformer of the Model
$this->response->item($user, new UserTransformer());
```

```php
// getting many Models
$users = $this->getUsers();
// building the response with the transformer of the Model
return $this->response->paginator($users, new UserTransformer());
```




<br/>
<a name="Inputs"></a>
###Inputs

`Inputs` are a great way to transfer the user input Data between the different components and automatically apply the Validation rules.

The `Input` has two main roles, firts to automatically validate the data against the defined rules and second to serve the data anywhere in the App.
<br/>
It's highly recommended to follow this pattern to transfer the application input data across the application code, to ensure the data is valid and never lost.
<br/>
Every `Input` MUST have `rules` and `get` functions, both returning Arrays.

`Input` code sample:

```php
namespace Mega\Modules\Authentication\Inputs;

use Mega\Infrastructure\Abstracts\Input;

class LoginInput extends Input
{
    public function rules()
    {
        return [
            'email'    => 'required|email|max:30',
            'password' => 'required|max:20',
        ];
    }

    public function get()
    {
        return [
            'email'    => $this->request['email'],
            'password' => $this->request['password'],
        ];
    }
}
```

**Input usage:**
To use an Input you simply inject it anywhere you want. In this example The `LoginInput` is injected in a `Command`:

```php
public function handle(LoginInput $loginInput)
{
	// display the input, after validating it
	echo $loginInput->get();
}
```



<br/>
<a name="Repositories"></a>
###Repositories

`Repositories` are the implementation of the Repository Design Pattern.

`Repositories` save and retrieve `Models` to or from the underlying storage mechanism.
<br/>
The `Repository` is used to separate the logic that retrieves the data and maps it to a `Model`, from the business logic that acts on the `Model`.
<br/>
Every `Model` SHOULD have a `Repository`. 
<br/>
You MUST not directly access a `Model` to perform any query. Instead, you SHOULD do this through the Repository.
<br/>
Every `Repository` MUST have a `model` function that returns the corresponding `Model`.
<br/>
By extending the extends `BaseRepository` you already have many ready functions to use, no need to write them manually (like `find`, `create`, `update` and much more).


`Repository` code sample:

```php
namespace Mega\Modules\User\Repositories\Eloquent;

use Mega\Modules\User\Contracts\UserRepositoryInterface;
use Mega\Infrastructure\Abstracts\BaseRepository;
use Mega\Modules\User\Models\User;

class UserRepository extends BaseRepository implements UserRepositoryInterface
{
    public function model()
    {
        return User::class;
    }
}
```

**Repository usage:**

```php
$users = $userRepository->paginate(10);
```

<br/>
<a name="Criterias"></a>
###Criterias

`Criterias` are a way to change the `Repository` of the query by applying specific conditions according to your needs.

Every `Criteria` must have an `apply` function.
<br/>
For more information about the `Criteria` read [this](https://github.com/andersao/l5-repository#create-a-criteria).



`Criteria` code sample:

```php
namespace Mega\Modules\User\Criterias\Eloquent;

use Mega\Infrastructure\Abstracts\Criteria;
use Prettus\Repository\Contracts\RepositoryInterface;

class OrderByCreationDateDescendingCriteria extends Criteria
{
    public function apply($model, RepositoryInterface $repository)
    {
        return $model->orderBy('created_at', 'desc');
    }
}
```

**Criteria usage:**

```php
$userRepository->pushCriteria(new OrderByCreationDateDescendingCriteria);
```

<br/>
<a name="Exceptions"></a>
###Exceptions

`Exceptions` are classes to be thrown in case of errors.

The `Exception` should have two properties `httpErrorCode` and `message`, both properties will be displayed when an error occurs.


`Exceptions` code sample:

```php
namespace Mega\Modules\Authentication\Exceptions;

use Mega\Infrastructure\Abstracts\ApiException;
use Symfony\Component\HttpFoundation\Response;

class AuthenticationFailedException extends ApiException
{
    public $httpErrorCode = Response::HTTP_UNAUTHORIZED;

    public $message = 'Credentials Incorrect.';
}
```

**`Exception` usage:**
`Exceptions` can be thrown from anywhere in the application.

```php
throw new AuthenticationFailedException();
```

You can override the $message property

```php
throw new AuthenticationFailedException('I am the error details');
```

```php
try {
	//...
} catch(Exception $e) {
	throw new AuthenticationFailedException($e->getMessage());
}
```


<br/>
<a name="Tests"></a>
###Tests

The main types of `Tests` recommended by this Architecture are Unit and Functional. (but you can write any other type of Tests such as Integration and Acceptance if you want).

Unit Tests SHOULD be in the `Modules` layer (each `Module` has it's own `Tests`). 
<br/>
And Functional Tests SHOULD be in the `Interfaces` layer (each `Interface` has it's own `Tests`).


Functional `Test` code samples:

```php
namespace Mega\Interfaces\Api\Tests;

use Illuminate\Foundation\Testing\DatabaseMigrations;
use Mega\Infrastructure\Abstracts\TestCase;

class LoginEndpointTest extends TestCase
{
    use DatabaseMigrations;
    use HelperTrait;
    
    private $endpoint = '/api/login';

    public function testLoginNonExistingUser()
    {
        $response = $this->call('POST', $this->endpoint, [
            'email'    => 'i-do-not-exist@mail.dev',
            'password' => 'secret',
        ]);

        $this->assertEquals($response->getStatusCode(), '401');

        $this->assertResponseContainKeyValue([
            'message' => 'Credentials Incorrect.',
        ], $response);
    }
}
```

Test Function:

```php
	// ...

    public function testLoginExistingUser()
    {
        $name = 'Mega';
        $email = 'mega@mail.dev';
        $password = 'secret';

        $userDetails = [
            'name'     => $name,
            'email'    => $email,
            'password' => $password,
        ];

        $this->registerAndLoginUser($userDetails);

        $response = $this->call('POST', $this->endpoint, [
            'email'    => $email,
            'password' => $password,
        ]);

        $this->assertEquals($response->getStatusCode(), '200');

        $this->assertResponseContainKeyValue([
            'email' => $email,
            'name'  => $name
        ], $response);

        $this->assertResponseContainKeys(['id', 'token'], $response);
    }
```

**Useful Tests Functions:** (available in all your `Tests`)

- ```assertResponseContainKeys($keys, $response)```
- ```assertResponseContainValues($values, $response)```
- ```assertResponseContainKeyValue($data, $response)```



<br/>

___




## Help us improve!
Let's help Developers write better code. For an easier Developers life :)


## Credits

- [Mahmoud Zalt](https://github.com/Mahmoudz)
- [All Contributors](../../contributors)

## License
MIT









