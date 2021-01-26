# Laravel Query Filter

A minimal approach to filtering eloquent models through query params

## Installation

```
composer require baraveli/laravel-query-filters
```

## Usage

### Create custom filter class in your laravel application

**In this case I have a filter for the user model. Every filter should extends the baraveli `QueryFilter` class.
And the method name in each filter class correspond to the url param name so in this case when you call ?search="ssd" it is actually calling the `search()` method**

```php
<?php

namespace App\Filters;

use Illuminate\Database\Eloquent\Builder;
use Baraveli\QueryFilters\QueryFilter;

class UsersFilters extends QueryFilter
{        
    /**
     * per_Page
     *
     * @param  mixed $number
     * @return void
     */
    public function per_Page($number = 5)
    {
        return $this->builder->paginate($number);
    }

    /**
     * search
     *
     * @param  mixed $search
     * @return Builder
     */
    public function search($search) : Builder
    {
        return $this->builder->where('name','like', '%' . $search .'%');
    }

    
}
```

### Add filterable trait to your eloquent model

**Adding this trait will give you a `filter()` scope with in your laravel model that you can use to apply the previous query filter**

```php

<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Baraveli\QueryFilters\Filterable;

class User extends Authenticatable
{
    use HasFactory, Notifiable, Filterable;
}

```

### Response 

**Now with in your controller or route return and apply the filters**

```php
Route::get('users', function(UsersFilters $usersfilter){
    return User::filter($usersfilter)->get();
});
```

Now you will be able to use the query filters like so

- /api/users?search=Ms : Search all the users names
- /api/users?search=Ms&per_page=5 : Search all the users names and paginate the results
