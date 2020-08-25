# Javascript

## Introduction

If you build your own Vue components it makes sense to know about the helpers
available in Javascript.

## Axios

The javascript global `axios` is configured with the route prefix specified in
the config **lit.php**. It is recommended to set a try around the function when
executing an axios request.

```javascript
async func() {
    let response;
    try {
        response = await axios.get('my-route')
    } catch(e) {
        return
    }

    ...
}
```

::: tip

For requests that shouldn't use the admin `route_prefix` the global `_axios` can
be used.

:::

## Helpers

In javascript and all Vue components the global variable `Lit` is available.
This includes the following helpers:

-   `baseURL`
-   `config`

### `Lit.baseURL`

The `route_prefix` set in config **lit.php**, with a front slash before and
after the prefix.

```php
// config/lit.php
'route_prefix' => 'admin' // Lit.baseURL would be /admin/
```

```javascript
<component>
	<a href="`${Lit.baseURL}my-link`">Link</a>
</component>
```

### `Lit.config`

All attributes from the config **lit.php**.

## Event Bus

The event bus can be called via the global Lit like this:

```javascript
Lit.bus.$on(event, callback);
Lit.bus.$once(event, callback);
Lit.bus.$off(event, callback);
Lit.bus.$emit(event, callback);
```

## Events

Some useful events:

-   `saved` - Executed when saved.
-   `saveCanceled` - When save has been canceled.
