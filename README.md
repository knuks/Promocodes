# Promocodes

[![PR0M0C0D35](http://i.imgsafe.org/c1512bb.jpg)](https://github.com/trexology/promocodes)

[![Latest Stable Version](https://poser.pugx.org/trexology/promocodes/version.png)](https://packagist.org/packages/trexology/promocodes)
[![Total Downloads](https://poser.pugx.org/trexology/promocodes/d/total.png)](https://packagist.org/packages/trexology/promocodes)
[![License](https://poser.pugx.org/trexology/promocodes/license)](https://packagist.org/packages/trexology/promocodes)

Promotional Codes Generator for [Laravel 5.*](http://laravel.com/)

## Table of Contents
- [Installation](#installation)
    - [Composer](#composer)
    - [Laravel](#laravel)
- [Usage](#usage)
    - [Recomendations](#recomendations)
    - [Methods](#methods)
- [Config 'n Migration](#config-n-migration)
- [License](#license)

## Installation

### Composer

Run composer command in your terminal.

    composer require trexology/promocodes

### Laravel

Please read [Config 'n Migration](#config-n-migration) section first. It's requried to create **promocodes** table

Open `config/app.php` and find the `providers` key. Add `PromocodesServiceProvider` to the array.

```php
Trexology\Promocodes\PromocodesServiceProvider::class
```

Find the `aliases` key and add `Facade` to the array. 

```php
'Promocodes' => Trexology\Promocodes\Facades\Promocodes::class
```

## Usage

### Recomendations

Run `php artisan make:model Promocode` and update `app/Promocode.php` as following:

```php
/**
 * @var bool
 */
public $timestamps = false;

/**
 * @var array
 */
protected $fillable = [
	'code',
	'reward',
	'is_used',
];

/**
 * @var array
 */
protected $casts = [
	'is_used' => 'boolean',
];
```

### Methods

You can generate Promotional codes using `generate` method.

The only parameter is amount of codes to generate.


```php
Promocodes::generate(5); // $amount = 1
```

- **$amount** int - number of promotional codes to be generated

This method will return array of codes with 5 element

---

You can generate and save codes instantly in your database using:

```php
Promocodes::save(5, 10.50); // $amount = 1, $reward = null
```

- **$amount** int - number of promotional codes to be generated
- **$reward** double - amount of reward of each promocodes

This will generate 5 codes and insert in your DB.

---

Check code using method `check`.

Method returns boolean.

```php
$valid = Promocodes::check('TEST-CODE'); // $promocode
```

- **$promocode** string - promotional code wich will be checked if issets

---

Laslty use code using method `apply`.

Method returns boolean.

```php
$applied = Promocodes::apply('TEST-CODE', true); // $promocode, $hard_check = false
```

- **$promocode** string - promotional code wich will be checked if issets, and applied to current user
- **$hard_check** boolean - if false or null, you will get only boolean value of checked promocode. If true you will get amount of reward as double or false.

If method returns false, code was already used or it wasn't valid

## Config 'n Migration

Publish Promocodes config & migration file using command:

```
php artisan vendor:publish
```

Created file `config\promocodes.php`. Inside you can change configuration as you wish.
Created migration file, now you can simply run `php artisan migrate` and that's it, you will have promocodes table.

## License

Promocodes is an open-sourced laravel package licensed under the MIT license

## TODO
- [x] Create migration for vendor publish
- [ ] Create Promocode Model trait
