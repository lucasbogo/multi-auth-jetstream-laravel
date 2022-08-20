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

#### Copiar Lógica da Model User

```
<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Fortify\TwoFactorAuthenticatable;
use Laravel\Jetstream\HasProfilePhoto;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens;
    use HasFactory;
    use HasProfilePhoto;
    use Notifiable;
    use TwoFactorAuthenticatable;

    /**
     * The attributes that are mass assignable.
     *
     * @var string[]
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array
     */
    protected $hidden = [
        'password',
        'remember_token',
        'two_factor_recovery_codes',
        'two_factor_secret',
    ];

    /**
     * The attributes that should be cast.
     *
     * @var array
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];

    /**
     * The accessors to append to the model's array form.
     *
     * @var array
     */
    protected $appends = [
        'profile_photo_url',
    ];
}
```
#### Colar na Model Admin

```
<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Fortify\TwoFactorAuthenticatable;
use Laravel\Jetstream\HasProfilePhoto;
use Laravel\Sanctum\HasApiTokens;

class Admin extends Authenticatable 
{
    use HasApiTokens;
    use HasFactory;
    use HasProfilePhoto;
    use Notifiable;
    use TwoFactorAuthenticatable;

    /**
     * The attributes that are mass assignable.
     *
     * @var string[]
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array
     */
    protected $hidden = [
        'password',
        'remember_token',
        'two_factor_recovery_codes',
        'two_factor_secret',
    ];

    /**
     * The attributes that should be cast.
     *
     * @var array
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];

    /**
     * The accessors to append to the model's array form.
     *
     * @var array
     */
    protected $appends = [
        'profile_photo_url',
    ];
}
```
### Migrar: 

#### Rodar comando:  ``` php artisan migrate ```

#### Criar **FACTORY** Admin: ``` php artisan make:factory AdminFactory ```

#### Copiar *atributos função* de UserFactory  ```public function definition() ``` 

```
  public function definition()
    {
        return [
            'name' => $this->faker->name(),
            'email' => $this->faker->unique()->safeEmail(),
            'email_verified_at' => now(),
            'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
            'remember_token' => Str::random(10),
        ];
    }
```

#### Colar em AdminFactory e alterar nome e email conforme sua necessidade
```
<?php

namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;
use App\Models\Team;
use Illuminate\Support\Str;
use Laravel\Jetstream\Features;

/**
 * @extends \Illuminate\Database\Eloquent\Factories\Factory<\App\Models\Admin>
 */
class AdminFactory extends Factory
{
    /**
     * Define the model's default state.
     *
     * @return array<string, mixed>
     */
    public function definition()
    {
        return [
            'name' => 'Admin',
            'email' => 'admin@gmail.com',
            'email_verified_at' => now(),
            'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
            'remember_token' => Str::random(10),
        ];
    }
}
```
### Efetuar o *SEED* da table Admin
- Ir para /database/seeders/DatabaseSeeder.php
- Descomentar a função que vem por *DEFAULT*
- Alterar User p/ Admin
- Tirar *factory(10)* que vem por default

#### Resultado:
```
<?php

namespace Database\Seeders;

// use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {
        // \App\Models\User::factory(10)->create();

        // \App\Models\User::factory()->create([
        //     'name' => 'Test User',
        //     'email' => 'test@example.com',
        // ]);
    }
}
```
#### Rodar comando *SEEDER*: ```php artisan migrate --seed```

---------------------------------------------------------------------------------------------------------------------------------------------------------

# CRIAR **GUARD** P/ ADMIN

Os *guardas* definem como os usuários são autenticados para cada solicitação. Por exemplo, o Laravel vem com um protetor de sessão que mantém o estado usando armazenamento de sessão e cookies. Os provedores definem como os usuários são recuperados de seu armazenamento persistente.

#### Ressalto que o *guard* User vem por *DEFAULT* ao instalar o Jetstream com Livewire, portanto, criei um *guard* admin apenas copiando a lógica *guard User*.

#### toda lógica, confirme mencionado no começo destee tutorial está na pasta **config/auth.php**

- Neste arquivo encontra-se todos os *Guards*.
- É possível configurar novos *Guards*. Que é o que farei neste tutorial, pois trata-se de multiautenticação User / Admin
- O *Guard* **web** vem por **DEFAULT**. Ou seja, se for trabalhar com um só tipo de autenticação, entaõ está configuração padrão basta.

## Configuração Guard Admin

#### Entrar na em: ``` config -> auth.php```
#### Criar um guard admin copiando. Basta copiar o guard *web* e alterar *web* para **admin** no segundo campo do auth.php
```
  'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        'admin' => [
            'driver' => 'session',
            'provider' => 'admins',
        ],
    ],

```
#### Mesmo processo, mas agora, para o terceito campo do auth.php
```
 'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\Models\User::class,
        ],

        'admins' => [
            'driver' => 'eloquent',
            'model' => App\Models\Admin::class,
        ],

        // 'users' => [
        //     'driver' => 'database',
        //     'table' => 'users',
        // ],
    ],
```

#### Mesmo processo, mas agora, para o quarto campo do auth.php, passwords
```
 'passwords' => [
        'users' => [
            'provider' => 'users',
            'table' => 'password_resets',
            'expire' => 60,
            'throttle' => 60,
        ],
        'admins' => [
            'provider' => 'admins',
            'table' => 'password_resets',
            'expire' => 60,
            'throttle' => 60,
        ],
    ],

```
#### Para ter um entendimento melhor sobre a MultiAutenticação Jetstream, basta rodar o comando ```php artisan r:l``` . Isso mostrá uma lista de todas as rotas e métodos que vem por default. Muitas delas serão utilizados neste trabalho.

## Proxima etapa:
### Configurar FortifyServiceProvider.php localizado na pasta **Providers**

#### O que é Providers?

Provedores de serviço Laravel padrão

Vamos começar com os provedores de serviço padrão incluídos no Laravel, eles estão todos na pasta app/Providers:

   - AppServiceProvider
   - AuthServiceProvider
   - Provedor de serviço de transmissão
   - EventServiceProvider
   - Provedor de serviço de rota

São todas classes PHP, cada uma relacionada ao seu tópico: "app" geral, Auth, Broadcasting, Events e Routes. Mas todos eles têm uma coisa em comum: o método boot().

Dentro desse método, você pode escrever qualquer código relacionado a uma dessas seções: auth, events, routes, etc. Em outras palavras, Service Providers são apenas classes para registrar alguma funcionalidade global.

Eles são separados como "provedores" porque são executados muito cedo no Ciclo de Vida do Aplicativo, portanto, é conveniente algo global aqui antes que o script de execução chegue aos Modelos ou Controladores.











