# On Boarding Kit

Use this as a Laravel project starting point. It contains packages and helpful modifications that are useful for your Laravel development.

(This project starter is based on the tips learned by watching Nuno Maduro on YouTube.)

### What is installed?

- Laravel 12 with no starter kit
    - Uses the default database SQLite
- Laravel Pint (already comes with Laravel)
    - Added configuration file called `pint.json`
- Pest PHP testing framework (XDebug must be installed to use code coverage capability)
    - Laravel plugin
- Larastan code analysis tool (It's like TypeScript for PHP)
    - Added configuration file called `phpstan.neon`

### Changes to `composer.json`

Allows you to run linting and testing commands using composer as opposed to calling `vendor/bin/<some_program>`

```json
{
    ...
        "scripts": {
            ...,
            "fix": [
                "@test:types",
                "npm run lint:fix",
                "pint"
            ],
            "lint": [
                "pint",
                "npm run lint"
            ],
            "test:lint": [
                "pint --test",
                "npm run test:lint"
            ],
            "test:unit": "pest --parallel --coverage --recreate-databases",
            "test:types": [
                "phpstan --memory-limit=512M"
            ],
            "test": [
                "@php artisan config:clear --ansi",
                "@test:unit",
                "@test:lint",
                "@test:types"
            ]
        },
    ...
}
```

### Changes to package.json

Allows you to run helpful npm commands for linting.

```json
...
    "scripts": {
        ...,
        "lint": "prettier --check",
        "lint:fix": "prettier --write .",
        "test:lint": "prettier --check .",
        "postinstall": "composer fix"
    }
...
```

### Changes to `AppServiceProvider.php`

Various helpful configuration changes.

```php

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        $this->configureCommands();
        $this->configureModels();
        $this->configureDates();
        $this->configureVite();
    }

    /**
     * Configure the application's commands
     */
    private function configureCommands(): void
    {
        DB::prohibitDestructiveCommands(
            $this->app->isProduction(),
        );
    }

    /**
     * Configure the application's models.
     */
    private function configureModels(): void
    {
        Model::shouldBeStrict();

        Model::unguard();
    }

    /**
     * Configure the application's Vite.
     */
    private function configureVite(): void
    {
        Vite::usePrefetchStrategy('aggressive');
    }

    /**
     * Configure the application's dates.
     */
    private function configureDates(): void
    {
        Date::use(CarbonImmutable::class);
    }
```
