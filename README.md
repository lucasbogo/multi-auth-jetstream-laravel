<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>


# Sobre:
[documentação oficial](https://jetstream.laravel.com/2.x/introduction.html)

Multi autenticação Jetstream User | Admin. Trata-se de um pacote incial de aplicações.
Este pacote, após devidamente configurado, providencia toda a lógica por trás do **login**, **register**, **recover_password**... Ou seja, o desenvolvedor não precisa, necessariamente, se preocupar como funciona esta lógica, linha por linha, no primeiro momento. A idéia principal, portanto, é agilizar e ajudar o programador a criar algo novo e incrível. Depois, após a criação e implentação, cabe ao desenvolvedor entender como funciona, realmente, a multiautenticação JetStream.

## Estutura de arquivos Laravel/Jetstream:

A lógica por trás deste pacote é encontrado em: 

- config -> fortify.php
- config -> jetstream.php

## Views:

As views referente a login e register podem ser encontradas, na newmodern-store, em:
- resources/views/auth

## Lógica login:

O Login e register está visivel pela a seguinte rota no web.php da newmodern-store. ressalto que esta rota vem por *default* no jetstream. Basta utilizar...

```
// login, registration, email verification, two-factor authentication, session management 
Route::middleware([
    'auth:sanctum', config('jetstream.auth_session'), 'verified'
])->group(function () {
    Route::get('/dashboard', function () {

        return view('dashboard');
    });
```

----------------------------------------------------------------------------------------------------------------------------------------------------------
# TUTORIAL

### Instalação:

#### Entrar no diretorio do projeto:

```
cd <nome_projeto>
```

#### Instalar Jetstream com Livewire :

```
php artisan jetstream:install livewire
```

#### Finalizar a instalação :

```
npm install && npm run dev
```

#### Migrar para o BD :

```
php artisan migrate
```

#### Rodar a Seeder :

```
php artisan db:seed
```

#### Criar controller Admin

```
php artisan make:controller Admin
```

#### Model + Migrations Table

```
php artisan make:model Admin -m
```

#### Copiar Atributos do User migrations_table (que vem por default ao instalar Jetstream) 

```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->foreignId('current_team_id')->nullable();
            $table->string('profile_photo_path', 2048)->nullable();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('users');
    }
};
```

#### Colar atributos em Admin migrations_table e alterar dados para casa com Admin

```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('admins', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->foreignId('current_team_id')->nullable();
            $table->string('profile_photo_path', 2048)->nullable();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('admins');
    }
};
```



