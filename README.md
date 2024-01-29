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



