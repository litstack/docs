# Rehearsal

## Introduction

An [orchestra](https://github.com/orchestral/testbench) extension to simplify
testing packages in a litstack application environment.

## Installation

Install the package via composer:

```shell
composer require --dev litstack/rehearsal
```

## Usage

To run tests in a litstack environment, your test class only needs to extend
`Litstack\Rehearsal\TestCase`.

```php
use Litstack\Rehearsal\TestCase as LitstackTestCase;

class TestCase extends LitstackTestCase
{
    public function test_litstack_is_installed()
    {
        $this->assertTrue(lit()->installed());
    }
}
```

This testcase is an extension of `Orchestra\Testbench\TestCase`. All its
functions can be used. Read more on how to test your packages in the
[orchestra docs](https://github.com/orchestral/testbench)
