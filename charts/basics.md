# Charts

## Introduction

Hello

Charts can be used to visualize model data and give the user an overview of
whats going on in his application in a selected time period. Charts can be
displayed on dashboards, index pages as well as on crud detail pages.

## Create via Artisan

Each chart has its own config file. The artisan command `lit:chart {name}`
generates the chart config into `./lit/app/Config/Charts`.

```shell
php artisan lit:chart SalesCountChart --area
```

When no type is given as a flag the default chart type `area` will be generated.

```shell
php artisan lit:chart MyChart --area
# Creates an area chart.

php artisan lit:chart MyChart --bar
# Creates a bar chart.

php artisan lit:chart MyChart --donut
# Creates a donut chart.

php artisan lit:chart MyChart --number
# Creates a number chart.

php artisan lit:chart MyChart --progress
# Creates a progress chart.
```

The command `lit:chart` creates a chart config in `./lit/app/Config/Charts`.

With the `--model` option you can directly specify the name of the model:

```shell
php artisan lit:chart MyChart --model=Booking
```

## Configure

Next your chart needs to be configured. Every chart type needs its corresponding
Model class and a title.

```php
use App\Models\Sale;

public $model = Sale::class;

public function title()
{
    return 'Sales Count';
}
```

Additionally, each chart type needs a specific configuration, which are linked
below:

-   [Area Chart](area.md)
-   [Bar Chart](bar.md)
-   [Donut Chart](donut.md)
-   [Number Chart](number.md)
-   [Progress](progress.md)

### Display Relationship Data

In some cases you may want to display the data of relations on a detail page of
a crud model. To do so, you need to specify the name of the relation for the
model in the relation property.

Let's assume that you want to display the number of bookings related to a user
and have created an **area** chart with the name `UserBookingsChart`. The config
for this would look as follows:

With the `--relation` flag the **property** is placed in the class when
generating the chart config.

```shell
php artisan lit:chart UserBookingsChart --model=User --relation=bookings
```

```php{lit/app/Config/Charts/UserBookingsChart.php}
<?php

namespace Lit\Config\Charts;

use Ignite\Chart\Chart;
use Ignite\Chart\Config\AreaChartConfig;

class UserBookingsChart extends AreaChartConfig
{
    public $model = \App\Models\User::class;

    public $relation = 'bookings';

    public function title()
    {
        return 'User Bookings';
    }

    public function value($query)
    {
        return $this->count($query);
    }
}
```

## Register Charts

Charts can be registered on pages that extend the `Ignite\Page\Page` class. For
example: `CrudShow` and `CrudIndex`.

```php
use Lit\Config\Charts\SalesCountChart;

$page->chart(SalesCountChart::class);
```

:::tip

Use a [form](../crud/forms.md) as a dashboard by only displaying charts on it.

:::

### Customize Card

Each chart is displayed in a card. You may customize them to your needs.

#### Variant

You may display the card in 3 different variants: `primary`, `secondary`,
`white`

```php
use Lit\Config\Charts\SalesCountChart;

$container->chart(SalesCountChart::class)->variant('primary');
$container->chart(SalesCountChart::class)->variant('secondary');
$container->chart(SalesCountChart::class)->variant('white');
```

![Chart Variants](./screens/variants.png)

#### Width

The width of the chart is indicated by `width`.

```php
use Lit\Config\Charts\SalesCountChart;

$container->chart(SalesCountChart::class)->width(1 / 3);
```

#### Height

The height of the chart is indicated by `height`.

```php
use Lit\Config\Charts\SalesCountChart;

$container->chart(SalesCountChart::class)->height('100px');
```

## Number Formatting

In the `mount` method the chart can be configured. You can set **format**,
**prefixes**, **suffixes** and **currencies**.

```php
public function mount(Chart $chart)
{
    $chart->format('0,0')->prefix('$ ')->suffix(' cm');

    // The currency method adds the needed format and suffix:
    $chart->currency('€');
}
```
