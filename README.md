Jetstream Cashier Billing Portal
================================

![CI](https://github.com/renoki-co/jetstream-cashier-billing-portal/workflows/CI/badge.svg?branch=master)
[![codecov](https://codecov.io/gh/renoki-co/jetstream-cashier-billing-portal/branch/master/graph/badge.svg)](https://codecov.io/gh/renoki-co/jetstream-cashier-billing-portal/branch/master)
[![StyleCI](https://github.styleci.io/repos/320252661/shield?branch=master)](https://github.styleci.io/repos/320252661)
[![Latest Stable Version](https://poser.pugx.org/renoki-co/jetstream-cashier-billing-portal/v/stable)](https://packagist.org/packages/renoki-co/jetstream-cashier-billing-portal)
[![Total Downloads](https://poser.pugx.org/renoki-co/jetstream-cashier-billing-portal/downloads)](https://packagist.org/packages/renoki-co/jetstream-cashier-billing-portal)
[![Monthly Downloads](https://poser.pugx.org/renoki-co/jetstream-cashier-billing-portal/d/monthly)](https://packagist.org/packages/renoki-co/jetstream-cashier-billing-portal)
[![License](https://poser.pugx.org/renoki-co/jetstream-cashier-billing-portal/license)](https://packagist.org/packages/renoki-co/jetstream-cashier-billing-portal)

Jetstream Cashier Billing Portal is a simple scaffolding billing portal to manage subscriptions, invoices and payment methods, built on top of Jetstream & Cashier Register.

Currently, only Inertia with Stripe are supported. For Paddle and/or Livewire, any PR is welcomed!

![example](example.png)

## 🤝 Supporting

Renoki Co. on GitHub aims on bringing a lot of open source projects and helpful projects to the world. Developing and maintaining projects everyday is a harsh work and tho, we love it.

If you are using your application in your day-to-day job, on presentation demos, hobby projects or even school projects, spread some kind words about our work or sponsor our work. Kind words will touch our chakras and vibe, while the sponsorships will keep the open source projects alive.

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/R6R42U8CL)

## 🚀 Installation

This package assumes you have installed Jetstream in your project. If not, head over to [Jetstream website](https://jetstream.laravel.com) for installation steps.

You can install the package via composer:

```bash
composer require renoki-co/jetstream-cashier-billing-portal
```

You shall install the Cashier Billing Portal in one command, just like Jetstream. This will install Cashier, Cashier Register and Billing Portal.

```bash
$ php artisan billing-portal:install
```

Next up, you should use the [custom Cashier Register trait](https://github.com/renoki-co/cashier-register#preparing-the-model) instead of Cashier's one for your billable model.

```php
use RenokiCo\CashierRegister\BillableWithStripe;

class User extends Model
{
    use BillableWithStripe;

    //
}
```

You will also have to [prepare the plans](https://github.com/renoki-co/cashier-register#preparing-the-plans) in `CashierRegisterServiceProvider`.

Import the created `app/Providers/CashierRegisterServiceProvider` class into your `app.php`:

```php
$providers = [
    // ...
    \App\Providers\CashierRegisterServiceProvider::class,
];
```

## 🙌 Usage

In `CashierRegisterServiceProvider`'s boot method you may define the plans you need:

```php
use RenokiCo\CashierRegister\CashierRegisterServiceProvider as BaseServiceProvider;
use RenokiCo\CashierRegister\Saas;

class CashierRegisterServiceProvider extends BaseServiceProvider
{
    /**
     * Boot the service provider.
     *
     * @return void
     */
    public function boot()
    {
        parent::boot();

        // Define plans here.
    }
}
```

By default, the subscriptions are accessible under `/user/billing/subscription`, but you can change the `/user/billing` prefix easily in `config/billing-portal.php`.

For more information about defining plans and quotas, check [Cashier Register documentation](https://github.com/renoki-co/cashier-register) and check [Laravel Cashier for Stripe documentation](https://laravel.com/docs/8.x/billing) on handling the billing.

## Custom Billables

By default, the billing is made directly on the currently authenticated model. In some cases like using billable trait on the Team model, you may change the model that will be retrieved from the current request. You may define it in the `boot()` method of `CashierRegisterServiceProvider`:

```php
use Illuminate\Http\Request;
use RenokiCo\BillingPortal\BillingPortal;

class CashierRegisterServiceProvider extends BaseServiceProvider
{
    /**
     * Boot the service provider.
     *
     * @return void
     */
    public function boot()
    {
        parent::boot();

        BillingPortal::setBillableOnRequest(function (Request $request) {
            return $request->user()->currentTeam;
        });
    }
}
```

## 🐛 Testing

``` bash
vendor/bin/phpunit
```

## 🤝 Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## 🔒  Security

If you discover any security related issues, please email alex@renoki.org instead of using the issue tracker.

## 🎉 Credits

- [Alex Renoki](https://github.com/rennokki)
- [All Contributors](../../contributors)
