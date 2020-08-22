# laravel10
kernal.php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    /**
     * The application's global HTTP middleware stack.
     *
     * These middleware are run during every request to your application.
     *
     * @var array
     */
    protected $middleware = [
        
        \App\Http\Middleware\TrustProxies::class,
        \App\Http\Middleware\CheckForMaintenanceMode::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
    ];

    /**
     * The application's route middleware groups.
     *
     * @var array
     */
    protected $middlewareGroups = [
        
        'web' => [
            
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            // \Illuminate\Session\Middleware\AuthenticateSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
            \App\Http\Middleware\CustomAuth::class,
        ],

        'api' => [
            'throttle:60,1',
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],
    ];

    /**
     * The application's route middleware.
     *
     * These middleware may be assigned to groups or used individually.
     *
     * @var array
     */
    protected $routeMiddleware = [
        
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'password.confirm' => \Illuminate\Auth\Middleware\RequirePassword::class,
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
        'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
    ];

    /**
     * The priority-sorted list of middleware.
     *
     * This forces non-global middleware to always be in the given order.
     *
     * @var array
     */
    protected $middlewarePriority = [
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\Authenticate::class,
        \Illuminate\Routing\Middleware\ThrottleRequests::class,
        \Illuminate\Session\Middleware\AuthenticateSession::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
        \Illuminate\Auth\Middleware\Authorize::class,
    ];
}
list,blade,php
@extends('layout')
@section('content')
<div>
<h1>List of Hotels</h1>
@if(Session::get('status'))
<div class="alert alert-success alert-dismissible fade show" role="alert">
 {{(Session::get('status'))}}
  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
    <span aria-hidden="true">&times;</span>
  </button>
</div>
@endif
<table class="table">
  <thead>
    <tr>
      <th scope="col">id</th>
      <th scope="col">Name</th>
      <th scope="col">Gmail</th>
      <th scope="col">Address</th>
      <th>Operations</th>
    </tr>
  </thead>
  <tbody>
  @foreach($data as $item)
    <tr>
      <th scope="row">{{$item->id}}</th>
      <td><a href="#">{{$item->name}}</a></td>
      <td>{{$item->gmail}}</td>
      <td>{{$item->address}}</td>
      <td><a href="delete/{{$item->id}}"><i class="fa fa-trash"></i></a></td>
      <td><a href="edit/{{$item->id}}"><i class="fa fa-edit"></i></a></td>
    </tr>
    @endforeach
  </tbody>
</table>


edit,blade.php
</div>
@stop
@extends('layout')
@section('content')
<div class="col-sm-6">
<h1>Edit Hotels</h1>
<form method="post" action="/edit">
@csrf
  <div class="form-group">
    <label>Name</label>
    <input type="hidden" value="{{$data->id}}" name="id">
    <input type="text" class="form-control" value="{{$data->name}}" placeholder="Enter name" name="name">
  </div>
  <div class="form-group">
    <label>Email address</label>
    <input type="text" class="form-control" value="{{$data->gmail}}"  placeholder="Enter email" name="gmail">
  </div><div class="form-group">
    <label>Address</label>
    <input type="text" class="form-control" value="{{$data->address}}" placeholder="Enter address" name="address">
  </div>
  
  <button type="submit" class="btn btn-primary">Submit</button>
</form>



</div>
@stop
register
@extends('layout')
@section('content')
<div>
<h1>Use Register Page</h1>

<div class="col-sm-6">
<form method="post" action="register">
@csrf
  <div class="form-group">
    <label>Name</label>
    <input type="text" class="form-control"  placeholder="Enter name" name="name">
  </div>
  <div class="form-group">
    <label>Gmail</label>
    <input type="text" class="form-control"  placeholder="Enter gmail" name="gmail">
  </div>
  <div class="form-group">
    <label>Password</label>
    <input type="password" class="form-control"  placeholder="Enter password" name="password">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
</div>
</div>
@stop
web.php
<?php

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

/*Route::get('/', function () {
    return view('welcome');
});

/*Route::get('/youtube/{id}', function ($id) {

    return view('youtube',["id"=>$id]);
});
Route::redirect('/here','/');
*/
//Route::post('/usercontroller','Users@index');
//Route::view('/mobiles','mobile');

Route::group(['middleware'=>"web"],function(){
    Route::get('/','Hotels@index');
    Route::get('/list','Hotels@list');
    Route::post('/add','Hotels@add');
    Route::view('add','add');
    Route::get('/delete/{id}','Hotels@delete');
    Route::get('/edit/{id}','Hotels@edit');
    Route::post('edit','Hotels@update');
    Route::view('register','register');
    Route::post('register','Hotels@register');
    Route::view('login','login');
    Route::post('login','Hotels@login');
});
custom.php
<?php

namespace App\Http\Middleware;

use Closure;
use Session;
class CustomAuth
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        //echo "hem";
        $path=$request->path();
        if(($path=="login" || $path=="register") && Session::get('user'))
        {
            return redirect('/');
        }
        else if(($path!="login" && !Session::get('user')) &&($path!="register" && !Session::get('user')))
        {
            return redirect('login');
        }
        return $next($request);
    }
}
logon
@extends('layout')
@section('content')
<div>
<h1>Use Register Page</h1>

<div class="col-sm-6">
<form method="post" action="login">
@csrf
  
  <div class="form-group">
    <label>Gmail</label>
    <input type="text" class="form-control"  placeholder="Enter gmail" name="gmail">
  </div>
  <div class="form-group">
    <label>Password</label>
    <input type="password" class="form-control"  placeholder="Enter password" name="password">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
</div>
</div>
@stop

