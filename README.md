# On Boarding Kit

Use this as a Laravel project starting point. It contains packages and helpful modifications that are useful for your Laravel development.

(This project starter is based on the tips learned by watching Nuno Maduro on YouTube.)

### What is installed?
* Laravel 12 with no starter kit
    * Uses the default database SQLite
* Laravel Pint configuration file called ```pint.json```

### Changes to ```composer.json``` 
Allows you to run linting and testing commands using composer as opposed to calling ```vendor/bin/<some_program>```
```json
{
    ...
        "scripts": {
            ...,
            "lint": "pint",
            "test:lint": "pint --test",
            "test": [
                "@php artisan config:clear --ansi",
                "@test:lint",
            ]
        },
    ...
}
```
