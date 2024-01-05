# Laravel

[Markdown syntax cheat sheet](https://www.markdownguide.org/cheat-sheet/)

## App building tips

Designing a class diagram before starting to code helps us understand the application’s scope and identify important data. It also helps us know how the application elements are related.

![DB schema](/images/db-schema.png)

> “I tend to do much of my thinking in the place where the cost of change and the cost of mistakes is as low as it can be: the whiteboard.” Newman, S. - Building microservices

## Starting Laravel project

1. Open Herd
2. Open TablePlus
3. Open Dev folder `cd ~/Documents/Dev/`
4. `laravel new app-name`
5. Access it via app-name.test URI in browser
6. `cd app-name/`
7. Migrate database `php artisan migrate`

## Laravel Project Structure

- **app/Http/Controllers/\***: we will place the app controllers here.
- **app/Models/\***: we will place the app models here.
- **database/migrations/\***: we will define the app migrations (the app's data- base schema definition) here.
- **public/\***: we will store our CSS, JavaScript, and images files here. The public folder also contains the index.php file, which is the entry point to the application.
- **resources/views/\***: we will place the app views here.
- **routes/web.php**: the web.php file will contain all the route definitions for the web application.
- **storage/app/public/\***: here, we will store the user-generated files, such as product images, that should be publicly accessible.
- **vendor/\***. The /vendor folder contains all libraries downloaded from Composer. The libraries/dependencies are listed in the composer.json file.
- **.env**: contains some common configuration values that may differ based on whether your application is running locally or on a production web server. It includes information such as database name, database username, and database password, among others.
- **composer.json**: holds metadata relevant to the project and manages the project's dependencies, scripts, version, and many more.

## Model-view-controller (MVC)

Software architectural pattern commonly used to develop web applications containing user interfaces.

- **Model** contains the business logic of the application. For example, the Online Store application product data and its functions.
- **View** contains the application’s user interface. For example, a view to register products or users.
- **Controller** acts as an interface between model and view elements. For example, a product controller collects information from a “create product” view and passes it to the product model to be stored in the database.

![Model-view-controller (MVC)](/images/mvc.png)

- Clients (users of our application e.g., browsers in mobile/desktop devices) connect to the application through the Hypertext Transfer Protocol (HTTP). HTTP gives users a way to interact with our web application.
- On the right, we have the server where we place our application code.
- All client interactions first pass for a route file called _web.php_.
- The _web.php_ file passes the interaction to a controller.
- Controllers communicate with models and pass information to the views, which are finally delivered to the clients as HTML, CSS, and JavaScript code.

We have four models (entities) corresponding to the classes defined in our class diagram.

## Blade

Blade is a powerful templating engine that is included with Laravel.

- Blade template files use the .blade.php file extension and are typically stored in the _resources/views_ directory.
- In your blade files, you will have a mix of HTML code with Blade directives and Blade elements.
- Blade directives are convenient shortcuts for common PHP control structures, such as conditional statements and loops.

```
@if (count($records) > 0)
I have records!
@else
I don't have any records!
@endif
```

**Do not use plain PHP code in your views.**

The main reason is to avoid using PHP syntax or PHP tags inside your view files. Instead, you should use the templating engine directives or helpers. What is the advantage? The template engines limit the number of available functionalities implemented in views (to provide a proper code separation).

If you don’t find a directive or helper for a functionality you need to implement in a view file, it is because that functionality should not be implemented in the view (maybe it should be implemented in a controller or in another file).

```
@yield('subtitle', 'A Laravel Online Store')
```

_@yield_ is used as a marker. We will inject code in those markers from child Blade views (using the _@section_ directive). _@yield_ uses two parameters, the first is the marker identifier. The second is a default value that will be injected if a child view does not inject code for that marker.

```
@extends('layouts.app')
@section('title', 'Home Page - Online Store')
@section('content')
<div class="text-center">
    Welcome to the application
</div>
@endsection
```

> Check [app.blade.php](app.blade.php) & [welcome.blade.php](welcome.blade.php)

The welcome view extends the _layouts.app_ view. This view injects the _'Home Page - Online Store'_ message in the _@yield('title')_ of the layouts.app and injects an HTML div with a welcome message inside the _@yield('content')_ of the _layouts.app_.

Curly braces are used in Blade files to display data passed to the view or invoke Laravel helpers.

```
{{ asset('/css/app.css') }}
```

Laravel includes a variety of global helper PHP functions. For example, the asset function generates a URL for an asset using the current scheme of the request (HTTP or HTTPS).

## Routing

Laravel Routing is a mechanism used to route all your application requests to specific methods or functions which will deal with those specific requests. Laravel routes accept a URI (Uniform Resource Identifier) along with a closure.

Closures are PHP’s version of anonymous functions. A closure is a function you can pass around as an object, assign to a variable, or pass as a parameter to other functions and methods.

Laravel routes are defined in your route files (located in the routes directory).

- The _routes/web.php_ file defines routes for your web interface. These are the routes that we will use in this book.
- The _routes/api.php_ file defines routes for your API (if you have one). These are routes used in service-oriented architectures or REST APIs

```php
Route::get('/', function () {
    return view('welcome');
});

Route::get('/', function () {
    $viewData = [];
    $viewData["title"] = "Home Page - Online Store";
    return view('home.index')->with("viewData", $viewData);
});

Route::get('/about', 'App\Http\Controllers\HomeController@about')->name("home.about");
```

> Check [web.php](web.php)

- The first route connects the “/” URI with a closure that returns a view (in this case, the _home.index_ view). _view()_ is a Laravel helper method which retrieves a view instance. Check how we pass the _viewData_ variable to the _home.index_ view by chaining the _with_ method onto the _view_ helper method.
- The second route connects the “/about” URI with the _HomeController about_ method. Besides, we define a custom route name by chaining the _name_ method onto the route definition.

## Controllers

Defining all your request handling logic inside in your route files’ closures does not seem smart. You will end with hundreds or thousands of code lines inside the route files (which affects the project maintainability). A good strategy is to organize this behavior using “controller” classes. Controllers can group related request handling logic into a single class. For example, a UserController class might handle all incoming requests related to users, including showing, creating, updating, and deleting users.

```php
public function index()
{
    $viewData = [];
    $viewData["title"] = "Home Page - Online Store";
    return view('home.index')->with("viewData", $viewData);
}
public function about()
{
    $data1 = "About us - Online Store";
    $data2 = "About us";
    $description = "This is an about page ...";
    $author = "Developed by: Your Name";
    return view('home.about')->with("title", $data1)->with("subtitle", $data2)->with("description", $description)->with("author", $author);
}
```

> Check [HomeController.php](HomeController.php)

- The _index_ method is connected to the (“/”) route in the _routes/web.php_ file. This method defines a set of variables and passes them to the _home.about_ view.
- When a user goes to the (“/about”) route, the _home.about_ view will be displayed (delivered in the _HomeController about_ method).

## Model

### Eloquent

[Eloquent: Getting Started](https://laravel.com/docs/10.x/eloquent)

Laravel object-relational mapper (ORM) that makes it super easy to interact with our database. When using Eloquent, each database table has a corresponding “Model” used to interact with that table. Eloquent models allow you to insert, retrieve, update, and delete records from the database tables.

Laravel models are located in the _app/Http/Models_ folder.

### Creating model

```
php artisan make:model Product
```

You will see the _Product.php_ file inside the _app/Models_ folder.

```php
<?php

namespace App\Models;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    use HasFactory;
}
```

- Eloquent will also assume that each model's corresponding database table has a primary key column named _id_. For all our migrations, we will use the _id_ method which defines an _id_ column.
- Eloquent will assume the _Product_ model stores records in the _products_ table (check the additional ‘s’). This convention applies to all models.
- By default, Eloquent expects _created_at_ and _updated_at_ columns to existing on your model's corresponding database table. Eloquent will automatically set these column's values when models are created or updated. For all our migrations, we will use the _timestamps_ method which creates these columns.

### Key methods

- **Product::all()**: retrieve all product records.
- **Product::find(1)**: retrieve the product with id 1.
- **Product::findOrFail(1)**: it is similar to the previous one, but it will throw an exception if no result is found.
- **Product::create(['name' => 'TV', ...])**: create a new record in the database. You must pass an associative array with the data to be assigned, id is not required since it is autogenerated.
- **Product::destroy(1)**: remove the product with id 1.

## Terminal commands

```
php --version # PHP version

composer --version # Composer version

which command-name  # locating the executable file matching the given command

pwd # print working directory

ll  # directory content

cd .. # parent directory

cd ~/Documents/Dev/ # change directory to Dev

cd app-name/ # change directory

laravel new app-name # create new Laravel project

php artisan migrate # database migration of Laravel project

php artisan make:model Name

cat file-name # output content of a file
```

## PHP_CodeSniffer

```
composer require --dev squizlabs/php\_codesniffer
```

```
./vendor/bin/phpcs
```

```
./vendor/bin/phpcbf
```

## MySQL

```
php artisan make:migration create\_products\_table
```

```
php artisan migrate
```

**Renaming MySQL database**

```
mysqldump -h 127.0.0.1 -P 3306 -u root -p online_store > olddbdump.sql
mysqladmin -h 127.0.0.1 -P 3306 -u root -p create laravel_onlinestore
mysql -h 127.0.0.1 -P 3306 -u root -p laravel_onlineshop < olddbdump.sql
```
