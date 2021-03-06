Chargify Wrapper for Laravel 5.+
=====================================

This is a wrapper using the chargley chargify SDK v0.1.4. It creates a service provider and facade for autoloading into laravel.

How to Install
---------------

### Laravel 5.+

1.  Install the `nikjaysix/laravel-chargify` package

    ```shell
    $ composer require nikjaysix/laravel-chargify
    ```

2. Update `config/app.php` to activate the LaravelChargify package

    ```php
    # Add `LaravelChargifyServiceProvider` to the `providers` array
    'providers' => array(
        ...
        NikJaySix\LaravelChargify\LaravelChargifyServiceProvider::class,
    )

    # Add the `LaravelChargifyFacade` to the `aliases` array
    'aliases' => array(
        ...
        'Chargify' => NikJaySix\LaravelChargify\LaravelChargifyFacade::class
    )
    ```

3.  Generate a Chargify config file

    ```shell
    $ php artisan vendor:publish
    ```

4.  Update `app/config/chargify.php` with your chargify API credentials

    ```php
    return array(
        'hostname' => '****.chargify.com',
        'api_key' => 'chargify api key',
        'shared_key' => 'chargify shared key'
    );
    ```
    
Example Usage
---------------

Creating subscriptions with v1 of the API

```php

$chargify = App::make('chargify');
  
// $handle is the subscription handle created in chargify
$new_subscription = $chargify->subscription()
    ->setProductHandle($handle)
    ->setCustomerAttributes([
        'first_name' => $user->first_name,
        'last_name' => $user->last_name,
        'email' => $user->email,
        'reference' => $user->id
    ])
    ->create();
  
if($new_subscription->isError()) {
    $errors = $new_subscription->getErrors();
    // handle errors
}

```

Retrieve a user's subscription

```php 

$chargify = App::make('chargify');  
  
$subscription = $chargify->subscription();
  
// $user_subscription->subscription_id is the subscription ID generated by chargify
$subscription->read($user_subscription->subscription_id);
  
if($subscription->isError()) {
    $errors = $subscription->getErrors();
    // handle errors
}
  
// store subscription data in session
foreach ($subscription as $key => $value) {
    session(['subscription_' . $key => $value]);
}
```
