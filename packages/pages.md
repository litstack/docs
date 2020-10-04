# Pages

<iframe src="https://github.com/sponsors/litstack/card" title="Sponsor litstack" height="88" width="600" style="border: 0;" class="github-sponsor"></iframe>

## Introduction

A package to turn your litstack application into a **CMS**. Add new pages to
your application via the litstack interface.

![pages](./screens/pages_screen.jpg 'pages')

## Sponsorware

Litstack pages was created by
**[Lennart Carstens-Behrens](https://twitter.com/lennartcb)** under the
**[Sponsorware license](https://github.com/sponsorware/docs)**.

## Setup

Add the Litstack repository to your application's composer.json file:

```json
"repositories": [
    {
        "type": "composer",
        "url": "https://store.litstack.io"
    }
],
```

Install the package via composer:

```shell
composer require litstack/pages
```

Publish the migrations and migrate:

```shell
php artisan vendor:publish --provider="Litstack\Pages\PagesServiceProvider"

php artisan migrate
```

## Setup a pages collection

With the artisan command lit:pages a new pages collection is created. For
example `blog`:

```shell
php artisan lit:pages Blog
```

A config is created and two controllers, one for the backend in
`./lit/app/Controllers/Pages` and one for your application in
`./app/Http/Controllers/Pages`.

In the config you can configure the route prefix and the possible repeatabels.
The url of the page consists of the route prefix specified in the config and the
sluggified page title. So a route for the following case could be
`/blog/my-title`. If the page is translatable a route is created for each locale
specified in the config like so:

-   `en/blog/{slug}`
-   `en/blog/{slug}`

```php{lit/app/Config/Pages/BlogConfig.php}
class BlogConfig extends PagesConfig
{
    ...

    public function appRoutePrefix(string $locale = null)
    {
        return "blog";
    }

    public function repeatables(Repeatables $rep)
    {
        // Build your repeatables in here.
    }
}
```

In the controller the page model is loaded with the method `getFjordPage`. This
can now be passed to a view like this:

```php{app/Http/Controllers/Pages}
use Litstack\Pages\ManagesPages;
use Illuminate\Http\Request;

class PagesController
{
    use ManagesPages;

    public function __invoke(Request $request, $slug)
    {
        $page = $this->getFjordPage($slug);

        return view('pages.blog')->withPage($page);
    }
}
```

## Route Field

To be able to select the pages in a route field you must first add them to a
route collection as described in the
[route field](../fields/route.md#register-routes) documentation.

FjordPages extends to Eloquent Collection with the helper method
`addToRouteCollection` that lets you add a list of pages directly to a route
collection:

```php
use Ignite\Crud\Fields\Route;
use Litsatck\Pages\Models\Page;

Route::register('app', function($collection) {
    Page::collection('blog')->get()->addToRouteCollection('Blog', $collection);
});
```
