```php
'guards' => [
  'web' => [
      'driver' => 'session',
      'provider' => 'usuarios',
  ],

  'api' => [
      'driver' => 'doctrine.token',
      'provider' => 'apis',
  ],
],

'providers' => [
    'apis' => [
        'driver' => 'doctrine.api',
    ],
    'usuarios' => [
        'driver' => 'doctrine.usuarios',
    ],
],
  ```
  
  ## AuthServiceProvider
  ```php
  Auth::provider('doctrine.api', function($app, array $config) {
            return new ProveedorApi($app->make(EntityManager::class));
        });

        Auth::extend('doctrine.token', function($app, $name, array $config) {
            return new GuardiaToken(
                $app->make(EntityManager::class),
                Auth::createUserProvider($config['provider']),
                $app->make('request')
            );
        });
  ```
  
  ## ProveedorApi
  ```php
  
class ProveedorApi implements UserProvider{

    /**
     * @var EntityManager
     */
    protected $em;

    public function __construct(EntityManager $em){
        $this->em = $em;
    }


    public function retrieveById($identifier)
    {
    }

    public function retrieveByCredentials(array $credentials)
    {

    }

    public function retrieveByToken($token, $tokenRecuerdo)
    {
        $qb = $this->em->createQueryBuilder();
        $qb->select('a');
        $qb->from(TokenApi::class, 'a');
        $qb->where($qb->expr()->eq('a.token', ':token'));
        $qb->setMaxResults(1);
        $qb->setParameter('token', $token);
        $token = $qb->getQuery()->getOneOrNullResult(Query::HYDRATE_OBJECT);
        return $token;
    }

    public function validateCredentials(Authenticatable $user, array $credentials)
    {
        $tokenAcceso = $user->getAuthPassword();
        $validacion = \Hash::check($credentials['tokenAcceso'], $tokenAcceso);
        return $validacion;
    }

    public function updateRememberToken(Authenticatable $user, $token)
    {
        $user->setRememberToken($token);
        $this->em->flush();
    }
}
```

## GuardiaToken
```php
<?php
/**
 * Autor: Manfred Juárez
 * Creado: 21/12/2019 - 22:04
 */

namespace App\Extensiones\Drivers\Auth\Token;


use Doctrine\ORM\EntityManager;
use App\Modelos\Seguridad\TokenApi;
use Illuminate\Contracts\Auth\Authenticatable;
use Illuminate\Contracts\Auth\Guard;
use Illuminate\Contracts\Auth\UserProvider;
use Illuminate\Http\Request;

class GuardiaToken implements Guard
{

    /**
     * @var EntityManager
     */
    protected $em;

    protected $proveedor;

    protected $solicitud;

    /** @var Authenticatable */
    protected $api;

    public function __construct(EntityManager $em, UserProvider $proveedor, Request $solicitud){
        $this->em = $em;
        $this->proveedor = $proveedor;
        $this->solicitud = $solicitud;
    }

    /**
     * Determine if the current user is authenticated.
     *
     * @return bool
     */
    public function check()
    {
        $apiToken = $this->solicitud->header('api_token');
        $accessToken = $this->solicitud->header('access_token');
        $this->api = $this->proveedor->retrieveByToken($apiToken, null);
        $acceso = $this->proveedor->validateCredentials($this->api, [
            'tokenAcceso' => $accessToken,
        ]);

        return $acceso;
    }

    /**
     * Determine if the current user is a guest.
     *
     * @return bool
     */
    public function guest()
    {
        return !$this->check();
    }

    /**
     * Get the currently authenticated user.
     *
     * @return \Illuminate\Contracts\Auth\Authenticatable|null
     */
    public function user()
    {
        return $this->api;
    }

    /**
     * Get the ID for the currently authenticated user.
     *
     * @return int|null
     */
    public function id()
    {
        // TODO: Implement id() method.
        return $this->api->getAuthIdentifier();
    }

    /**
     * Validate a user's credentials.
     *
     * @param array $credentials
     * @return bool
     */
    public function validate(array $credentials = [])
    {
        // TODO: Implement validate() method.
        dd($credentials);
    }

    /**
     * Set the current user.
     *
     * @param \Illuminate\Contracts\Auth\Authenticatable $user
     * @return void
     */
    public function setUser(Authenticatable $user)
    {
        // TODO: Implement setUser() method.
        $this->api = $user;
    }
}
```
  
## Autenticatable
  
  ```php
  <?php


namespace App\Modelos\Seguridad;
use App\Modelos\Planes\PlanAdquirido;
use Illuminate\Contracts\Auth\Authenticatable;

/**
 * Class TokenApi
 * @entity
 * @table(name="table")
 */
class TokenApi implements Authenticatable
{
    /**
     * @id
     * @var string
     * @column(type="string", name="token")
     */
    protected $token;

    /**
     * @var string
     * @column(type="string", name="token_acceso")
     */
    protected $tokenAcceso;

    /**
     * @var string
     * @column(type="string", name="token_recuerdo")
     */
    protected $tokenRecuerdo;

    /**
     * @var Usuario
     * @ManyToOne(targetEntity="App\Modelos\Seguridad\Usuario")
     * @JoinColumn(name="usuario", referencedColumnName="id_usuario")
     */
    protected $usuario;

    /**
     * @var PlanAdquirido
     * @ManyToOne(targetEntity="App\Modelos\Planes\PlanAdquirido")
     * @JoinColumn(name="plan", referencedColumnName="id_plan")
     */
    protected $plan;

    /**
     * @var int
     * @column(type="integer", name="ambiente")
     */
    protected $ambiente;

    /**
     * Get the name of the unique identifier for the user.
     *
     * @return string
     */
    public function getAuthIdentifierName()
    {
        return 'token';
    }

    /**
     * Get the unique identifier for the user.
     *
     * @return mixed
     */
    public function getAuthIdentifier()
    {
        return $this->token;
    }

    /**
     * Get the password for the user.
     *
     * @return string
     */
    public function getAuthPassword()
    {
        return $this->tokenAcceso;
    }

    /**
     * Get the token value for the "remember me" session.
     *
     * @return string
     */
    public function getRememberToken()
    {
        return $this->tokenRecuerdo;
    }

    /**
     * Set the token value for the "remember me" session.
     *
     * @param string $value
     * @return void
     */
    public function setRememberToken($value)
    {
        $this->tokenRecuerdo = $value;
    }

    /**
     * Get the column name for the "remember me" token.
     *
     * @return string
     */
    public function getRememberTokenName()
    {
        return null;
    }

    /**
     * @return string
     */
    public function getToken(): string
    {
        return $this->token;
    }

    /**
     * @param string $token
     */
    public function setToken(string $token): void
    {
        $this->token = $token;
    }

    /**
     * @return string
     */
    public function getTokenAcceso(): string
    {
        return $this->tokenAcceso;
    }

    /**
     * @param string $tokenAcceso
     */
    public function setTokenAcceso(string $tokenAcceso): void
    {
        $this->tokenAcceso = $tokenAcceso;
    }

    /**
     * @return string
     */
    public function getTokenRecuerdo(): string
    {
        return $this->tokenRecuerdo;
    }

    /**
     * @param string $tokenRecuerdo
     */
    public function setTokenRecuerdo(string $tokenRecuerdo): void
    {
        $this->tokenRecuerdo = $tokenRecuerdo;
    }

    /**
     * @return Usuario
     */
    public function getUsuario(): Usuario
    {
        return $this->usuario;
    }

    /**
     * @param Usuario $usuario
     */
    public function setUsuario(Usuario $usuario): void
    {
        $this->usuario = $usuario;
    }

    /**
     * @return PlanAdquirido
     */
    public function getPlan(): PlanAdquirido
    {
        return $this->plan;
    }

    /**
     * @param PlanAdquirido $plan
     */
    public function setPlan(PlanAdquirido $plan): void
    {
        $this->plan = $plan;
    }

    /**
     * @return int
     */
    public function getAmbiente(): int
    {
        return $this->ambiente;
    }

    /**
     * @param int $ambiente
     */
    public function setAmbiente(int $ambiente): void
    {
        $this->ambiente = $ambiente;
    }
}
```
