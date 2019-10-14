
## Laravel 5.4 Multi Auth using MD5

Laravel is a web application framework with expressive, elegant syntax. Laravel 5.4 Multi Auth implimentation with MD5 instead of bcrypt

## How to set up the project.
1 Open MySQL in your machine.<br/>
2	Create a blank  Database and name it as  'multiauth' or whatever name you want .<br/>
3	Go to project folder and open .env file.<br/>
4	Set your MySQL access credentials in .env file.<br/>
  ```
  DB_USERNAME='your db username'.
  DB_PASSWORD= 'your db password'
  ```
5 Run composer install<br/>
6	Go to project folder via command line and run migrations and seeder<br/>
 Type following command and hit enter.<br/>
  ```
  php artisan migrate
  php artisan db:seed --class=UsersTableSeeder 
  ```
7 Run the app using php artisan serve
  Server would be running at localhost:8000
  Try  login to application
```
User login username= user@gmail.com passowrd=123456
Admin login username  admin@gmail.com password =123456 Tick Admin checkbox
```
## Files to check.
1 app/Libraries/md5Hasher.php<br/>
2 config/app.php (check providers array for hashing)<br/>
3 config/auth.php (multi auth providers)<br/>
4 app/Http/Controllers/Auth/LoginController<br/>
5 app/User.php<br/>
6 app/Admin.php<br/>
7 migration and seeding<br/>
## Explanation 

1. A md5Hash library is added  app/Libraries/md5Hasher.php  which impliments Illuminate\Contracts\Hashing\Hasher
2 HashServiceProvider which extends ServiceProvider is added at app/providers folder
3 Two methods are added like follows
 
 ```
 public function register()
    {
        $this->app->singleton('hash', function () {
            return new md5Hasher;
        });
    }
    
   
     public function provides()
    {
        return ['hash'];
    }
 ```
3  Add  this to providers array for hashing (config/app.php )
```
          /*
         * md5 hash Service Providers...
         */
        App\Providers\HashServiceProvider::class,
```
4 set up guards for multiauth as below

```
'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],

        'api' => [
            'driver' => 'token',
            'provider' => 'users',
        ],
         'admin' => [
            'driver' => 'session',
            'provider' => 'admins',
        ],
        'admin-api' => [
            'driver' => 'token',
            'provider' => 'admins',
        ],
    ],
     'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\User::class,
        ],
'admins' => [
            'driver' => 'eloquent',
            'model' => App\Admin::class,
        ],
```
5 set up app/Http/Controllers/Auth/LoginController

```
 protected function authenticated(Request $request, $user)
    {
        if (get_class($user) == 'App\Admin') {
            return redirect()->intended('/admin');
        }
    }

    /**
     * Get the needed authorization credentials from the request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return array
     */
    protected function credentials(Request $request)
    {
        if ($request->input('type') == 'admin') {
            return [
                'login-name'    => $request->input('email', null),
                'password'      => $request->input('password', null),
            ];
        }
        return $request->only('email', 'password');
    }
 /**
     * Get the guard to be used during authentication.
     *
     * @return \Illuminate\Contracts\Auth\StatefulGuard
     */
    protected function guard(Request $request)
    {
        if ($request->input('type') == 'admin') {
            return Authendicator::guard('admin');
        } 
        return Authendicator::guard();
    }

    /**
     * Get the login username to be used by the controller.
     *
     * @return string
     */
    public function username()
    {
        $request = request();
        if ($request->input('type') == 'admin') {
            return 'login-name';
        }
        return 'email';
    }
    ```
    
    6 Model classes are created for user and Admin
    ```
    app/User.php
    app/Admin.php
    ```
    
