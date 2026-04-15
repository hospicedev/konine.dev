---
layout: documentation
title: Userguide
---
# Using the User Guide

The Userguide is a Konine module that renders documentation pages and an API browser directly inside your running application. It is intended for **local development only** — do not enable it on production servers.

## Enabling the module

Open your application's `application/bootstrap.php` and add `userguide` to the `Kohana::modules()` call:

```php
Kohana::modules(array(
    'userguide' => MODPATH.'userguide', // In-application user guide
    // ... your other modules
));
```

Once enabled, the guide is available at `http://localhost/guide` (or wherever your local app is served).

## Navigating the guide

- The **index page** (`/guide`) lists all modules that have documentation enabled.
- Each module's guide pages appear under `/guide/<modulename>/`.
- The **API browser** is at `/guide/api` and documents all classes visible to the Konine autoloader.

## Controlling what appears

You can disable individual modules from appearing in the guide by overriding their config in `application/config/userguide.php`:

```php
return array(
    'modules' => array(
        'orm' => array(
            'enabled' => FALSE,
        ),
    ),
);
```

To disable the API browser entirely, set `api_browser` to `FALSE` in the same file:

```php
return array(
    'api_browser' => FALSE,
);
```

See [Configuration](/documentation/userguide/config) for the full list of options.
