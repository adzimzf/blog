---
layout: post
title: "Lambda dan Closure di PHP"
categories: PHP
author: "adzim zul fahmi"
meta: "lambda"
---

Lambda dan closures baru di tambahkan pada `PHP versi 5.3`. Keduanya memberikan beberapa fitur dan kemampuan baru untuk mengubah koding kita  menjadi lebih rapih. Namun gw rasa masih banyak programmer yang tidak sadar tentang Lambda dan Closure atau mungkin malahan bingung apa itu sebernarnya.

Di postingan kali ini gw bakal mencoba menjelaskan tentang Lambda dan Closure  dan memberikan contoh potongan kode penggunanya.
![enter image description here](https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/PHP-logo.svg/1200px-PHP-logo.svg.png)

**Apa si sebenarnya Lambda itu?**
-----------------------------

Lambda adalah *anonymous function*  yang dapat di isikan kedalam variabel atau dijadikan sebagai parameter ke fungsi lain. Kalo lo sudah familiar dengan JavaScript atau Ruby pasti enggak bakalan asing lagi dengan yang namanya *anonymous function*.

*Anonymous Function* itu sendiri sebenarnya fungsi sederhana yang tanpa nama, sebagai contoh  untuk membuat sebuah fungsi lo bakal nulis seperti ini:

```php
//fungsi biasa
function greeting ()
{
return "hello world";
}
```

kemudian di panggil dengan cara:

```php
echo greeting();
```

Tapi jika anonymous function yang tanpa menggunakan nama, maka akan jadi seperti ini:

```php
//Anonymous function
function ()
{
return "Hello World";
{
```


**Menggunakan Lambda**
------------------

Karena enggak menggunakan nama maka harus di masukan ke dalam sebuah variabel atau sebagai parameter untuk fungsi lain.

```php
// Anonymous function
// assigned to variable
$greeting = function () {
  return "Hello world";
}
 
// Call function
echo $greeting();
// Returns "Hello world"
```

lo juga bisa gunakan sebagai parameter untuk fungsi lain:
```php
// Pass Lambda to function
function shout ($message){
  echo $message();
}
 
// Call function
shout(function(){
  return "Hello world";
});
```

**Mengapa menggunakan Lambda?**
---------------------------

Lambda sangat berguna karena dapat membuat fungsi yang haya sekali pake. Terkadang lo butuh fungsi yang hanya digunakan sekali, tapi lo bikin fungsi itu sebagai fungsi global. Daripada lo bikin fungsi yang hanya dipake sekali lebih baik menggunakan lambda.

**Apa itu Closure?**
----------------
Closure sebenarnya sama saja dengan Lambda cuman bedanya dia bisa menggunakan variable di luar fungsi itu sendiri.

Contohnya:
```php
// Create a user
$user = "Philip";
 
// Create a Closure
$greeting = function() use ($user) {
  echo "Hello $user";
};
 
// Greet the user
$greeting(); // Returns "Hello Philip"
```

Seperti yang lo liat di atas , Closure dapat menggunakan variabel ``$user`` karena sudah menggunakan keyword ``use``.

Jika lo ngubah variabel ``$user`` di dalam closure tidak akan mengubah variabel aslinya. Untuk mengubah variabel aslinya  kita dapat menambahkan tanda ``&`` sebelum variabel, contohnya:

```php
// Set counter
$i = 0;
// Increase counter within the scope
// of the function
$closure = function () use ($i){ $i++; };
// Run the function
$closure();
// The global count hasn't changed
echo $i; // Returns 0
 
// Reset count
$i = 0;
// Increase counter within the scope
// of the function but pass it as a reference
$closure = function () use (&$i){ $i++; };
// Run the function
$closure();
// The global count has increased
echo $i; // Returns 1
```

Closure sangat berguna ketika menggunakan fungsi php yang menerima callback seperti ``array_map``, ``array_filter``,`` array_reduce``, atau ``array_walk``.

fungsi ```array_walk``` mengambil value dari array dan mengembalikanya sebagai fungsi callback.
```php
// An array of names
$users = array("John", "Jane", "Sally", "Philip");
 
// Pass the array to array_walk
array_walk($users, function ($name) {
  echo "Hello $name<br>";
});
// Returns
// -> Hello John
// -> Hello Jane
// -> ..
```


dan lo bisa menggunakan variabel di luar Closure dengan keyword use.
```php
// Set a multiplier
$multiplier = 3;
 
// Create a list of numbers
$numbers = array(1,2,3,4);
 
// Use array_walk to iterate
// through the list and multiply
array_walk($numbers, function($number) use($multiplier){
  echo $number * $multiplier;
});
```

pada contoh di atas kayaknya enggak mungkin banget bikin fungsi yang hanya di gunakan untuk mengalikan 2 angka secara bersamaan. Jika lo bikin fungsi seperti ini, kemudian setelah beberapa bulan lo ngecek kodingan lo lagi pasti muncul pertanyaan "ngapain gw bikin fungsi yang dapat di akses global yang hanya digunakan sekali?". Dengan menggunakan Closure sebagai callback kita dapat gunain fungsi yang cuman dipake sekali saja.

**Contoh nyata penggunanya**
------------------------

Setelah kita mengerti apa itu Lambda Closure dan cara penggunanya yang dapat membuat koding kita lebih mudah dibaca. Beberapa contoh paling populer penggunaan Lambda dan Closure yaitu di Laravel:
```php
Route::get('user/(:any)', function($name){
  return "Hello " . $name;
});
```

Potongan kode diatas digunakan untuk membuat URL yang dapat di isi dengan macam-macam kata, misalnya `user/adzim` atau `user/bejo`.

Gw harap penjelasan di atas dapat dipahami dengan mudah apa itu Lambda dan Closure dan cara penggunanya dan semoga koding kita jadi semakin rapih dan mudah dibaca, Amiin.
