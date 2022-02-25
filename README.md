# Infobip API PHP SDK

[![Latest Stable Version](https://poser.pugx.org/fsasvari/infobip-api-php-sdk/v/stable)](https://packagist.org/packages/fsasvari/infobip-api-php-sdk)
[![Latest Unstable Version](https://poser.pugx.org/fsasvari/infobip-api-php-sdk/v/unstable)](https://packagist.org/packages/fsasvari/infobip-api-php-sdk)
[![Total Downloads](https://poser.pugx.org/fsasvari/infobip-api-php-sdk/downloads)](https://packagist.org/packages/fsasvari/infobip-api-php-sdk)
[![License](https://poser.pugx.org/fsasvari/infobip-api-php-sdk/license)](https://packagist.org/packages/fsasvari/infobip-api-php-sdk)

This is a PHP SDK for Infobip API and you can use it as a dependency to add [Infobip APIs](https://www.infobip.com/docs/api) to your application. To use this, you'll need an Infobip account. If you do not own one, you can create a [free account here](https://www.infobip.com/signup).

#### Table of contents:

- [General Info](#general-info)
- [License](#license)
- [Compatibility Chart](#compatibility-chart)
- [Installation](#installation)
- [Basic usage](#basic-usage)
  - [Example](#example)
  - [Exceptions](#exceptions)
  - [Laravel](#laravel)
  - [Symfony](#symfony)
- [Documentation](#documentation)
- [Development](#development)

## General Info

For `infobip-api-php-sdk` versioning we use [Semantic Versioning](https://semver.org) scheme.

## License

Published under [MIT License](LICENSE).

## Compatibility Chart

| Infobip API PHP SDK | PHP         |
|---------------------|-------------|
| 1.*                 | 7.2.5+ / 8+ |

## Installation

To start using the `infobip-api-php-sdk` library add it as dependency to your `composer.json` project dependency:

```sh
composer require fsasvari/infobip-api-php-sdk
```

Or you can add it manually to `composer.json` file:

```json
"require": {
    "fsasvari/infobip-api-php-sdk": "1.*"
}
```
And then simply run `composer install` to download dependencies.

## Basic usage

Example on how to create the `InfobipClient` instance. You can also define it in your DI Container and get configuration data from the `env()` or configuration file.

```php
$infobipClient = new Infobip\InfobipClient(
    'apiKey',
    'baseUrl',
    3 // timeout in seconds, optional parameter
);
```
### Example

A simple example of using the `InfobipClient` for calling the :

```php
// example 1
$resource = new \Infobip\Resources\WhatsApp\WhatsAppTextMessageResource(
    '441134960000',
    '441134960001'
);

$response = $infobipClient
    ->whatsApp()
    ->sendWhatsAppTextMessage($resource);
```

### Exceptions

There is a couple of Infobip `exceptions` that you could stumble upon while using the `InfobipClient`:

- Bad request (400)
- Unauthorized (401)
- Forbidden (403)
- Not found (404)
- Too many requests (429)
- Internal server error (500)

Of course, there is a way of handling those:

```php
try {
    $resource = new \Infobip\Resources\WhatsApp\WhatsAppTextMessageResource();
    
    $response = $infobipClient
        ->whatsApp()
        ->sendWhatsAppTextMessage($resource);
} catch (\Infobip\Exceptions\InfobipException $exception) {
    $exception->getMessage(); // error message
    $exception->getCode(); // http status code
    $exception->getValidationErrors(); // array of validation errors, only available on 400 Bad request exception
}
```

### Laravel

Register the `InfobipServiceProvider` in your `config/app.php` configuration file:

```php
'providers' => [
    // Application Service Providers...
    // ...

    // Other Service Providers...
    Infobip\Support\Laravel\InfobipServiceProvider::class,
    // ...
],
```

And then run the following command to copy the Infobip configuration file to your `config` directory:

```shell
php artisan vendor:publish --provider="Infobip\Support\Laravel\InfobipServiceProvider"
```

After that, you can start using the Infobip API PHP SDK package in your Laravel project, just inject the `InfobipClient` into your codebase:

```php
<?php

namespace App\Services;

use Infobip\InfobipClient;
use Infobip\Resources\WhatsApp\WhatsAppDownloadInboundMediaResource;

final class InfobipService
{
    public function dashboard(InfobipClient $infobipClient)
    {
        $resource = new WhatsAppDownloadInboundMediaResource(
            'sender',
            'mediaId'
        );
        
        $response = $infobipClient
            ->whatsApp()
            ->downloadWhatsAppInboundMedia($resource);
        
        return $response;
    }
}
```

### Symfony

TODO

## Documentation

Infobip API Documentation can be found [here](https://www.infobip.com/docs/api).

## Development

Feel free to participate in this open source project. These are some console commands that could help you with the development:

```sh
vendor/bin/phplint
vendor/bin/phpstan analyse
vendor/bin/php-cs-fixer fix src
vendor/bin/php-cs-fixer fix tests
vendor/bin/phpunit
```
