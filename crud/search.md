# Search

You can make models searchable and allow your user to quickly find what they are
looking for.

![search](./screens/search.png 'search')

## Setup

First you need to enable the search by adding the
`Ignite\Search\SearchServiceProvider` service provider to you `lit` config:

```php
// ...

'providers' => [
    // ...
    \Ignite\Search\SearchServiceProvider::class,
]
```

Now you choose the crud's that should be searchable and edit the displayed
search result by adding the `searchResult` method to the associated crud config:

```php
use App\Models\Order;
use Ignite\Search\Result;

class OrderConfig extends CrudConfig
{
    // ...

    public function searchResult(Result $result, Order $order)
    {
        $result
            ->title("Order #{$order->id}")
            ->description($order->products()->count().' Products.')
            ->hint("Created By {$order->user->name}")
            ->icon(fa('shopping-cart'));
    }
}
```

And finally register the crud config to your search in the created
`SearchConfig`:

```php
public function main(Search $search)
{
    $search
        ->add(OrderConfig::class)
        ->add(CustomerConfig::class);
}
```

## Image Preview

To add an image preview you may pass the image to the search result:

```php
public function searchResult(Result $result, User $user)
{
    $result
        ->title($user->name)
        ->description($user->email)
        ->image($user->profile_image);
}
```
