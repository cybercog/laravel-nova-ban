# Laravel Nova Ban

![cog-laravel-nova-ban](https://user-images.githubusercontent.com/1849174/28749192-fe2d2cb4-74c7-11e7-955e-9c48e81106c2.png)

## Introduction

Behind the scenes [cybercog/laravel-ban](https://https://github.com/cybercog/laravel-ban) is used.

## Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Prepare bannable model](#prepare-bannable-model)
  - [Prepare bannable model database table](#prepare-bannable-model-database-table)
  - [Register Ban Actions in Nova Resource](#register-ban-actions-in-nova-resource)
- [Contributing](#contributing)
- [Testing](#testing)
- [Security](#security)
- [Contributors](#contributors)
- [Alternatives](#alternatives)
- [License](#license)
- [About CyberCog](#about-cybercog)

## Installation

First, pull in the package through Composer:

```sh
$ composer require cybercog/laravel-nova-ban
```

## Usage

### Prepare bannable model

```php
use Cog\Contracts\Ban\Bannable as BannableContract;
use Cog\Laravel\Ban\Traits\Bannable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable implements BannableContract
{
    use Bannable;
}
```

### Prepare bannable model database table

Bannable model must have `nullable timestamp` column named `banned_at`. This value used as flag and simplify checks if user was banned. If you are trying to make default Laravel User model to be bannable you can use example below.

#### Create a new migration file

```sh
$ php artisan make:migration add_banned_at_column_to_users_table
```

Then insert the following code into migration file:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddBannedAtColumnToUsersTable extends Migration
{
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->timestamp('banned_at')->nullable();
        });
    }
    
    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropColumn('banned_at');
        });
    }
}
```

Apply new migration.

### Register Ban Actions in Nova Resource

Register `Ban` and `Unban` actions inside your `Bannable` Model's Resource.

```php
public function actions(Request $request)
{
    return [
        new Ban(),
        new Unban(),
    ];
}
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Testing

Run the tests with:

```sh
$ vendor/bin/phpunit
```

## Security

If you discover any security related issues, please email open@cybercog.su instead of using the issue tracker.

## Contributors

| <a href="https://github.com/antonkomarev">![@antonkomarev](https://avatars.githubusercontent.com/u/1849174?s=110)<br />Anton Komarev</a> |
| :---: |

[Laravel Nova Ban contributors list](../../contributors)

## Alternatives

*Feel free to add more alternatives as Pull Request.* 

## License

- `Laravel Nova Ban` package is open-sourced software licensed under the [MIT License](LICENSE) by Anton Komarev.
- `Fat Boss In Jail` image licensed under [Creative Commons 3.0](https://creativecommons.org/licenses/by/3.0/us/) by Gan Khoon Lay.

## About CyberCog

[CyberCog](http://www.cybercog.ru) is a Social Unity of enthusiasts. Research best solutions in product & software development is our passion.

- [Follow us on Twitter](https://twitter.com/cybercog)
- [Read our articles on Medium](https://medium.com/cybercog)

<a href="http://cybercog.ru"><img src="https://cloud.githubusercontent.com/assets/1849174/18418932/e9edb390-7860-11e6-8a43-aa3fad524664.png" alt="CyberCog"></a>
