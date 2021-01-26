# Laravel Query Filter

Minimal approach to filtering eloquent models using query params

## Usage

### Create custom filter class in your laravel application

In this case I have a filter for the user model. Every filter should extends the baravelu `QueryFilter` class.

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

Adding this trait will give you a `filter()` scope with in your laravel model that you can use to apply the previous query filter

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
