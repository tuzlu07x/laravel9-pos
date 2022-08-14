# Türk bankaları için sanal pos paketi (Laravel)

## Temel Paket
[Pos](https://github.com/tuzlu07x/laravel9-pos)

### Minimum Gereksinimler
- PHP >= 8.1
- ext-dom
- ext-json
- ext-openssl
- ext-SimpleXML

### Kurulum
```sh
composer require tuzlufatih/laravel-pos
```
`config/app.php` dosyasındaki `providers` kısmına aşağıdaki kodu ekleyin:
```php
'providers' => [
    // ...
    Mews\LaravelPos\LaravelPosServiceProvider::class,
]
```

`config/app.php` dosyasındaki `aliases` kısmına aşağıdaki kodu ekleyin:
```php
'aliases' => [
    // ...
    'LaravelPos' => Mews\LaravelPos\Facades\LaravelPos::class,
]
```

Konsolda, proje ana dizinindeyken aşağıdaki komut girilir:
```sh
php artisan vendor:publish --provider="tuzlufatih\LaravelPos\LaravelPosServiceProvider"
```

### Kullanım
```php

$pos = \Mews\LaravelPos\Facades\LaravelPos::instance();

$pos->account([
    'bank'          => 'garanti',
    'model'         => 'regular',
    'client_id'     => 'XXXXX',
    'username'      => 'XXXXX',
    'password'      => 'XXXXX',
    'env'           => 'test',
    'terminal_id'   => 'xxx',

]);

$order = [
    'id'            => 'unique-order-id-' . Str::random(16),
    'name'          => 'John Doe', // optional
    'email'         => 'mail@customer.com', // optional
    'user_id'       => '12', // optional
    'amount'        => (double) 100,
    'installment'   => '0',
    'currency'      => 'TRY',
    'ip'            => request()->ip(),
    'transaction'   => 'pay', // pay => Auth, pre PreAuth
];

$card = [
    'number'        => 'XXXXXXXXXXXXXXXX',
    'month'         => 'XX',
    'year'          => 'XX',
    'cvv'           => 'XXX',
];

$pos->prepare($order);

$payment = $pos->payment($card);

dd($payment->response);

```

License
----

MIT
