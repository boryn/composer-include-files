# Composer - Include Files Plugin

When using the Composer Autoloader if you need project files included prior to files autoloaded by any of your dependencies your out of luck. No longer!

## Installation

```bash
composer require 0.0.0/composer-include-files
```

## Usage

Just add the files you need included using `"include_files"` and they will be included prior to any files included by your dependencies.

```json
// composer.json (project)
{
    "extra": {
        "include_files": [
            "/path/to/file/you/want/to/include",
            "/path/to/another/file/you/want/to/include"
        ]
    },
}
```

Composer v2.2 includes a new security feature: https://getcomposer.org/doc/06-config.md#allow-plugins

*As of Composer 2.2.0, the allow-plugins option adds a layer of security allowing you to restrict which Composer plugins are able to execute code during a Composer run.*

So as well you need to add this library to the allowed plugins in your `composer.json` file like this:


```json
{
    "config": {
        "allow-plugins": {
            "0.0.0/composer-include-files": true
        }
    }
}
```

## Specific Use Case

A good example of where this is required is when overriding helpers provided by Laravel.

In the past simply modifying `bootstrap/autoload.php` to include helpers was sufficient. However new versions of PHPUnit include the Composer Autoloader prior to executing the PHPUnit bootstrap file. Consequently this method of overriding helpers is no longer viable as it will trigger a fatal error when your bootstrap file is included.

But now we can use *Composer - Include Files Plugin* to have Composer include the files in the necessary order.

```json
// composer.json (project)
{
    "require": {
        "laravel/framework": "^5.2",
        "funkjedi/composer-include-files": "^1.0",
    },
    "extra": {
        "include_files": [
            "app/helpers.php"
        ]
    },
}
```
