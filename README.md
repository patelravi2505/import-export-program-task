# import-export-program-task
import and export program step


wrrite down the import and export step 

step1 :-
1) composer require maatwebsite/excel

step 2 :- 
2) changes app.php file 
   i) providers :- maatwebsite\Excel\ExcelServiceProvider::class,
   
   ii) aliases :- 'Excel'=>maatwebsite\Excel\Facades\Excel::class

step 3 :- 
 
command :-  php artisan make:import UserImport --model=Models/User

userImport.php 
            'name'=>$row['0'],
            'email'=>$row['1'],
            'password'=>\Hash::make($row['2']),

step 4 :- 

command :-  php artisan make:Export UserExport --model=Models/User

step 5 :- 

create blade file :- import.blade.php
  <form action="{{route('import')}}" method="post" enctype="multipart/form-data">
        @csrf
        <input type="file" name="file" id="">
        <br>
        <br>
        <button>Import button</button>
    </form>
    <br><br>

    <a href="{{route('export')}}">export user data</a>

step 6 :- 

write down the method in controller 

use App\Exports\UserExport;
use App\Imports\UserImport;
use Maatwebsite\Excel\Facades\Excel;

  public function importExportView()
    {
        return view('import');
    }


    public function export(){
        return Excel::download(new UserExport,'users.xlsx');
    }

    public function import(){
        Excel::import(new UserImport,request()->file('file'));
        return back();
    }

Step 7 :- 
route web.php

Route::get('importExportView',[Usercontroller::class,'importExportView'])->name('importExportView');
Route::get('export',[Usercontroller::class,'export'])->name('export');
Route::post('import',[Usercontroller::class,'import'])->name('import');



# email notification 


first of all mail trape open and copy to code and set env file

After run tarminal cmd php artisan make:mail ravimail 

After the open the mail folder and insert the your blade file name in view

 public function content():Content{
        return new Content(
            view: 'ravi'           
        );
    }

Then after create a view file in ravi.blade.php 
After this blade route create 

route::('ravi','ravi')

Then after open the controller and register code to insert one line 

Mail::to($user['email'])->send(new ravimail());

after the run cmd in php artisan serve 
And 
your mail sucessfull send mail trap....


# crude


controller


<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;

class UserController extends Controller
{
   
    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
        $request->validate([
      
            'name'                 =>'required',
            'email'                =>'required|unique:users',
            'password'             =>'required|confirmed|min:6',
            'password_confirmation'=>'required'

        ]);

        $user = new User();
        $user->name=$request->name;
        $user->email=$request->email;
        $user->password=$request->password;
        $user->save();
        return view('login');


    }
  
    
    /**
     * check user login details.
     */
    public function login(Request $request){
        
       $data= $request->validate([
          
            'email'    =>'required',
            'password' =>'required|min:6',

        ]);

            if(Auth::attempt($data)){

                return redirect('dashboard');
            }else{
                return view('login');
        }
    }

    public function logout(){
        Auth::logout();
        return view('login');
    }

    /**
     * Display the specified resource.
     */
    public function show()
    {
        $user = User::all();
        return view('dashboard',compact('user'));
    }

    /**
     * Show the form for editing the specified resource.
     */
    public function edit($id)
    {
        $user = User::find($id);
            return view('edit',compact('user'));
        
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request)
    {
        $request->validate([
      
            'name'                 =>'required',
            'email'                =>'required|unique:users',
            'password'             =>'required|confirmed|min:6',
            'password_confirmation'=>'required'

        ]);

        $user = User::find($request->id);
        $user->name=$request->name;
        $user->email=$request->email;
        $user->password=$request->password;
        $user->save();
        return redirect('dashboard');


    
    }

    /**
     * Remove the specified resource from storage.
     */
    public function delete($id)
    {
        
        $user =User::find($id)->delete();
        return redirect('dashboard');
    }
}


#route::::


Route::view('signup','signup');
Route::view('login','login');
Route::view('dashboard','dashboard');
Route::post('signup',[UserController::class,'store']);
Route::post('login',[UserController::class,'login']);
Route::get('dashboard',[UserController::class,'show']);
Route::get('delete/{id}',[UserController::class,'delete']);
Route::get('edit/{id}',[UserController::class,'edit']);
Route::post('update',[UserController::class,'update'])->name('update');
Route::get('logout',[UserController::class,'logout'])->name('logout');


# view page

##signup.php

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <title>Document</title>
</head>
<body>
    <form action="" method="post">
        @csrf
    <section class="pt-5 pb-5 mt-0 align-items-center d-flex bg-dark" style="min-height: 100vh; background-size: cover; background-image: url(https://images.unsplash.com/photo-1477346611705-65d1883cee1e?ixlib=rb-0.3.5&amp;q=80&amp;fm=jpg&amp;crop=entropy&amp;cs=tinysrgb&amp;w=1920&amp;fit=max&amp;ixid=eyJhcHBfaWQiOjMyMDc0fQ&amp;s=c0d43804e2c7c93143fe8ff65398c8e9);">
        <div class="container-fluid">
          <div class="row  justify-content-center align-items-center d-flex-row text-center h-100">
            <div class="col-12 col-md-4 col-lg-3   h-50 ">
              <div class="card shadow">
                <div class="card-body mx-auto">
                  <h4 class="card-title mt-3 text-center">Create Account</h4>
                  <p class="text-center">Get started with your free account</p>
                  <p>
                    <a href="" class="btn btn-block btn-info">
                      <i class="fab fa-twitter mr-2"></i>Login via Google</a>
                    <a href="" class="btn btn-block btn-primary">
                      <i class="fab fa-facebook-f mr-2"></i>Login via facebook</a>
                  </p>
                  <p class="text-muted font-weight-bold ">
                    <span>OR</span>
                  </p>
                  <form>
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-user"></i> </span>
                      </div>
                      <input name="name" class="form-control" placeholder="Full name" type="text">
                    </div>
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-envelope"></i> </span>
                      </div>
                      <input name="email" class="form-control" placeholder="Email address" type="text">
                    </div>
                    @error('email')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-lock"></i> </span>
                      </div>
                      <input class="form-control" placeholder="Create password" name="password" type="password">
                    </div>
                    @error('password')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-lock"></i> </span>
                      </div>
                      <input class="form-control" placeholder="Repeat password" type="password" name="password_confirmation">
                    </div>
                    @error('password_confirmation')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
                    <div class="form-group">
                      <input type="submit" class="btn btn-primary btn-block">
                    </div>
                    <p class="text-center">Have an account?
                      <a href="login">Log In</a>
                    </p>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
     </section>

    </form>
</body>
</html>


#dashbord

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <title>Document</title>
</head>
<body>
    
    <a href="{{route('logout')}}" class="btn btn-info">logout</a>
    <br><br>
    <table class="table">
  <thead class="thead-dark">
    <tr>
      <th scope="col">id</th>
      <th scope="col">name</th>
      <th scope="col">email</th>
      <th scope="col">Action</th>

     
    </tr>
  </thead>
  <tbody>
   
    <tr>
        @foreach($user as $data)
      <td >{{$data['id']}}</td>
      <td>{{$data['name']}}</td>
      <td>{{$data['email']}}</td>
      <td><a href="{{'delete/'.$data['id']}}" class="btn btn-danger">delete</a>
      <a href="{{'edit/'.$data['id']}}" class="btn btn-primary">Edit</a></td>

      
    
    </tr>
    @endforeach 
  </tbody>

</table>


   
</body>
</html>

#login page

<!DOCTYPE html>
<html>
<head>
	<title>Login Page</title>
   <!--Made with love by Mutiullah Samim -->
   
	<!--Bootsrap 4 CDN-->
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
    
    <!--Fontawesome CDN-->
	<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.3.1/css/all.css" integrity="sha384-mzrmE5qonljUremFsqc01SB46JvROS7bZs3IO2EmfFsd15uHvIt+Y8vEf7N7fWAU" crossorigin="anonymous">
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css" rel="stylesheet" id="bootstrap-css">
<script src="//maxcdn.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
	<!--Custom styles-->
	<link rel="stylesheet" type="text/css" href="styles.css">
    <style>

html,body{
background-image: url('http://getwallpapers.com/wallpaper/full/a/5/d/544750.jpg');
background-size: cover;
background-repeat: no-repeat;
height: 100%;
font-family: 'Numans', sans-serif;
}

.container{
height: 100%;
align-content: center;
}

.card{
height: 370px;
margin-top: auto;
margin-bottom: auto;
width: 400px;
background-color: rgba(0,0,0,0.5) !important;
}

.social_icon span{
font-size: 60px;
margin-left: 10px;
color: #FFC312;
}

.social_icon span:hover{
color: white;
cursor: pointer;
}

.card-header h3{
color: white;
}

.social_icon{
position: absolute;
right: 20px;
top: -45px;
}

.input-group-prepend span{
width: 50px;
background-color: #FFC312;
color: black;
border:0 !important;
}

input:focus{
outline: 0 0 0 0  !important;
box-shadow: 0 0 0 0 !important;

}

.remember{
color: white;
}

.remember input
{
width: 20px;
height: 20px;
margin-left: 15px;
margin-right: 5px;
}

.login_btn{
color: black;
background-color: #FFC312;
width: 100px;
}

.login_btn:hover{
color: black;
background-color: white;
}

.links{
color: white;
}

.links a{
margin-left: 4px;
}
    </style>
</head>
<body>
    <br><br><br><br><br>
  
<div class="container">
	<div class="d-flex justify-content-center h-100">
		<div class="card">
			<div class="card-header">
				<h3>Sign In</h3>
				<div class="d-flex justify-content-end social_icon">
					<span><i class="fab fa-facebook-square"></i></span>
					<span><i class="fab fa-google-plus-square"></i></span>
					<span><i class="fab fa-twitter-square"></i></span>
				</div>
			</div>
			<div class="card-body">
            <form action="" method="post">
        @csrf
					<div class="input-group form-group">
						<div class="input-group-prepend">
							<span class="input-group-text"><i class="fas fa-user"></i></span>
						</div>
						<input type="email" name="email" class="form-control" placeholder="user Email">
						
					</div>
                    @error('email')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
					<div class="input-group form-group">
						<div class="input-group-prepend">
							<span class="input-group-text"><i class="fas fa-key"></i></span>
						</div>
						<input type="password" name="password" class="form-control" placeholder="password">
					</div>
                    @error('password')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
					<div class="row align-items-center remember">
						<input type="checkbox">Remember Me
					</div>
					<div class="form-group">
						<input type="submit"  class="btn float-right login_btn">
					</div>
				</form>
			</div>
			<div class="card-footer">
				<div class="d-flex justify-content-center links">
					Don't have an account?<a href="signup">Sign Up</a>
				</div>
				<div class="d-flex justify-content-center">
					<a href="#">Forgot your password?</a>
				</div>
			</div>
		</div>
	</div>
</div>

</body>
</html>

# edit page

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <title>Document</title>
</head>
<body>
    <form action="{{route('update')}}" method="post">
        @csrf
    <section class="pt-5 pb-5 mt-0 align-items-center d-flex bg-dark" style="min-height: 100vh; background-size: cover; background-image: url(https://images.unsplash.com/photo-1477346611705-65d1883cee1e?ixlib=rb-0.3.5&amp;q=80&amp;fm=jpg&amp;crop=entropy&amp;cs=tinysrgb&amp;w=1920&amp;fit=max&amp;ixid=eyJhcHBfaWQiOjMyMDc0fQ&amp;s=c0d43804e2c7c93143fe8ff65398c8e9);">
        <div class="container-fluid">
          <div class="row  justify-content-center align-items-center d-flex-row text-center h-100">
            <div class="col-12 col-md-4 col-lg-3   h-50 ">
              <div class="card shadow">
                <div class="card-body mx-auto">
                  <h4 class="card-title mt-3 text-center">Create Account</h4>
                  <p class="text-center">Get started with your free account</p>
                  <p>
                    <a href="" class="btn btn-block btn-info">
                      <i class="fab fa-twitter mr-2"></i>Login via Google</a>
                    <a href="" class="btn btn-block btn-primary">
                      <i class="fab fa-facebook-f mr-2"></i>Login via facebook</a>
                  </p>
                  <p class="text-muted font-weight-bold ">
                    <span>OR</span>
                  </p>
                  <form>
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-user"></i> </span>
                      </div>
                      <input type="hidden" name="id" id="" value="{{$user['id']}}">
                      <input name="name" class="form-control" placeholder="Full name" type="text" value="{{$user['name']}}">
                    </div>
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-envelope"></i> </span>
                      </div>
                      <input name="email" class="form-control" placeholder="Email address" type="text" value="{{$user['email']}}">
                    </div>
                    @error('email')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-lock"></i> </span>
                      </div>
                      <input class="form-control" placeholder="Create password" name="password" type="password" value="{{$user['password']}}">
                    </div>
                    @error('password')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <span class="input-group-text"> <i class="fa fa-lock"></i> </span>
                      </div>
                      <input class="form-control" placeholder="Repeat password" type="password" value="{{$user['password_confirmation']}}" name="password_confirmation">
                    </div>
                    @error('password_confirmation')
                    <strong style="color: red;">{{$message}}</strong>
                    @enderror
                    <div class="form-group">
                      <input type="submit" class="btn btn-primary btn-block">
                    </div>
                    <p class="text-center">Have an account?
                      <a href="login">Log In</a>
                    </p>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
     </section>

    </form>
</body>
</html>





