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

![Model-view-controller (MVC)](/images/mvc.png)

## Terminal commands

```
php --version

composer --version

pwd # print working directory

ll  # directory content

cd .. # parent directory

which command-name  # locating the executable file matching the given command

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
