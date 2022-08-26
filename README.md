# Laravel Nova Ban

![cog-laravel-nova-ban](https://user-images.githubusercontent.com/1849174/44558292-ffc0a080-a74b-11e8-8699-a79de865910d.png)

<p align="center">
<a href="https://github.com/cybercog/laravel-nova-ban/releases"><img src="https://img.shields.io/github/release/cybercog/laravel-nova-ban.svg?style=flat-square" alt="Releases"></a>
<a href="https://styleci.io/repos/145874233"><img src="https://styleci.io/repos/145874233/shield" alt="StyleCI"></a>
<a href="https://github.com/cybercog/laravel-nova-ban/blob/master/LICENSE"><img src="https://img.shields.io/github/license/cybercog/laravel-nova-ban.svg?style=flat-square" alt="License"></a>
</p>

## Introduction

Behind the scenes [cybercog/laravel-ban](https://github.com/cybercog/laravel-ban) is used.

![laravel-nova-ban-preview](https://user-images.githubusercontent.com/1849174/44547881-1b648080-a725-11e8-9472-bad1486c06f0.png)

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

Pull in the package through Composer.

```shell script
composer require cybercog/laravel-nova-ban
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

```shell script
php artisan make:migration add_banned_at_column_to_users_table
```

Then insert the following code into migration file:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddBannedAtColumnToUsersTable extends Migration
{
    public function up(): void
    {
        Schema::table('users', function (Blueprint $table) {
            $table->timestamp('banned_at')->nullable();
        });
    }
    
    public function down(): void
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
        new \Cog\Laravel\Nova\Ban\Actions\Ban(),
        new \Cog\Laravel\Nova\Ban\Actions\Unban(),
    ];
}
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Testing

Run the tests with:

```shell script
vendor/bin/phpunit
```

## Security

If you discover any security related issues, please email open@cybercog.su instead of using the issue tracker.

## Contributors

| <a href="https://github.com/antonkomarev">![@antonkomarev](https://avatars.githubusercontent.com/u/1849174?s=110)<br />Anton Komarev</a> | <a href="https://github.com/ sergiy-petrov ">![@ sergiy-petrov ](https://avatars.githubusercontent.com/u/8986207?s=110)<br />Sergiy Petrov</a> | <a href="https://github.com/mvdnbrk">![@mvdnbrk](https://avatars.githubusercontent.com/u/802681?s=110)<br />Mark van den Broek</a> | <a href="https://github.com/tooshay">![@tooshay](https://avatars.githubusercontent.com/u/703589?s=110)<br />Roy Shay</a> |
| :---: | :---: | :---: | :---: |

[Laravel Nova Ban contributors list](../../contributors)

## Alternatives

*Feel free to add more alternatives as Pull Request.* 

## License

- `Laravel Nova Ban` package is open-sourced software licensed under the [MIT License](LICENSE) by [Anton Komarev].
- `Fat Boss In Jail` image licensed under [Creative Commons 3.0](https://creativecommons.org/licenses/by/3.0/us/) by Gan Khoon Lay.

## About CyberCog

[CyberCog] is a Social Unity of enthusiasts. Research best solutions in product & software development is our passion.

- [Follow us on Twitter](https://twitter.com/cybercog)

<a href="https://cybercog.su"><img src="https://cloud.githubusercontent.com/assets/1849174/18418932/e9edb390-7860-11e6-8a43-aa3fad524664.png" alt="CyberCog"></a>

[Anton Komarev]: https://komarev.com
[CyberCog]: https://cybercog.su
