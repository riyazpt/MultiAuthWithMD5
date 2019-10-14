<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>


</p>

## Laravel 5.4 Multi Auth using MD5

Laravel is a web application framework with expressive, elegant syntax. Laravel 5.4 Multi Auth implimentation with MD5 instead of bcrypt

## How to set up the project.
1	Open MySQL in your machine.<br/>
2	Create a blank  Database and name it as  'multiauth' or whatever name you want .<br/>
3	Go to project folder and open .env file.<br/>
4	Set your MySQL access credentials.<br/>
  DB_USERNAME=your db username.<br/>
  DB_PASSWORD= your db password<br/>
5 Run composer install
6	Go to project folder via command line and run migrations and seeder<br/>
7	Type following command and hit enter.<br/>
  php artisan migrate
  php artisan db:seed --class=UsersTableSeeder <br/>
8 Run the app using php artisan serve
Server would be running at localhost:8000<br/>
Try to login to application

User login user@gmail.com/123456<br/>
Admin login admin@gmail.com/123456 Tick Admin checkbox<br/>

