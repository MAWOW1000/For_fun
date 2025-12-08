# Basic Routing

Every route maps a URI to a closure or controller action.

## Default Route Files

Laravel provides several default route files stored in the `routes` directory.

### `routes/web.php`

  * Used for pages that need sessions, CSRF protection, and the `web` middleware group.

### `routes/api.php`

  * Registered under the `/api` prefix.
  * **Stateless by default** (no sessions or CSRF).
  * To enable API features like authentication (Laravel Sanctum), run:
    ```bash
    php artisan install:api
    ```

-----

## Available Route Methods

Laravel supports all common HTTP verbs:

```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

### Shortcuts

If you need to handle multiple verbs for a single URI:

```php
// Match specific HTTP verbs
Route::match(['get', 'post'], '/uri', $callback);

// Match any HTTP verb
Route::any('/uri', $callback);
```

-----

## Dependency Injection

You can inject dependencies directly into route closures. Laravel resolves them automatically using the **Service Container**.

```php
use Illuminate\Http\Request;

Route::get('/user/{id}', function (Request $request, $id) {
    return $id;
});
```

-----

## CSRF Protection

  * **Required** for `web` routes (`routes/web.php`).
  * **Not used** in `routes/api.php`.
  * You must add the `@csrf` directive to your HTML forms:

<!-- end list -->

```blade
<form method="POST" action="/profile">
    @csrf
    </form>
```

-----

## Redirect Routes

You can define redirects directly in the route file:

```php
Route::redirect('/old', '/new');          // Returns 302 (Temporary)
Route::permanentRedirect('/old', '/new'); // Returns 301 (Permanent)
```

-----

## View Routes

If a route only returns a view, use the `Route::view` shortcut:

```php
// Syntax: Route::view('/uri', 'view.name', ['data' => 'value']);
Route::view('/welcome', 'welcome', ['name' => 'Dev']);
```

> **Note:** Parameters like `view`, `data`, `status`, and `headers` are reserved and cannot be used as route parameters in this method.

-----

## Listing Routes

To list all defined routes, use the Artisan command:

```bash
php artisan route:list
```

### Useful Flags

  * `--path=api`: Filter to show only API routes.
  * `-v`: Show middleware attached to routes.
  * `-vv`: Expand middleware groups to show specific middleware.
  * `--only-vendor`: View routes defined by vendor packages.

-----

## Custom Route Registration

You can customize how routes are loaded (e.g., in `bootstrap/app.php` or a Service Provider), but be aware that you may lose Laravel's defaults, such as automatic middleware groups or configuration settings.