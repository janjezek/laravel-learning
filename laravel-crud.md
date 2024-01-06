# Laravel CRUD

1. Create new Laravel app `laravel new crudposts`
2. Create a database
3. Update _.env_ file and link database
4. Create new migration `php artisan make:migration create_posts_table`
5. Open _yyyy_mm_dd_hhmmss_create_posts_table.php_ in _database/migrations_.
6. Define table columns in `up` function

```php
public function up()
{
  Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->text('body');
    $table->timestamps();
  });
}
```

https://laravel.com/docs/10.x/migrations#available-column-types

7. Create new table in database `php artisan migrate`
8. Create new controller `php artisan make:controller PostController --api` _PostController.php_ file in _app/Http/Controllers_
9. Update the `store` function that adds a post to the database

```php
public function store(Request $request)
{
    $request->validate([
        'title' => 'required|max:255',
        'body' => 'required',
    ]);

    Post::create($request->all());

    return redirect()->route('posts.index')
        ->with('success', 'Post created successfully.');
}
```

10. Update the `index` function that fetches all the posts from the database

```php
public function index()
{
    $posts = Post::all();

    return view('posts.index', compact('posts'));
}
```

11. Update the `udpate` function that contains the _id_ of the post to update

```php
public function update(Request $request, $id)
{
    $request->validate([
        'title' => 'required|max:255',
        'body' => 'required',
    ]);

    $post = Post::find($id);
    $post->update($request->all());

    return redirect()->route('posts.index')
        ->with('success', 'Post updated successfully.');
}
```

12. Udpate the `destroy` function finds a post with the given _id_ and deletes it from the database

```php
public function destroy($id)
{

    $post = Post::find($id);
    $post->delete();

    return redirect()->route('posts.index')
        ->with('success', 'Post deleted successfully');
}
```

13. Update the `create` function renders the _resources/views/posts/create.blade.php_ page, which contains the form for adding posts to the database

```php
public function create()
{
    return view('posts.create');
}
```

14. Update the `show` function finds a post with the given _id_ in the database and renders the _resources/views/posts/show.blade.php_ file with the post

```php
public function show($id)
{
    $post = Post::find($id);

    return view('posts.show', compact('post'));
}
```

15. Update the `edit` function finds a post with the given _id_ in the database and renders the _resources/views/posts/edit.blade.php_ file with the post details inside a form

```php
public function edit($id)
{
    $post = Post::find($id);

    return view('posts.edit', compact('post'));
}
```

16. Create model `php artisan make:model Post`
