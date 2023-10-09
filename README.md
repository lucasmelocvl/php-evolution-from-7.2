# php-evolution-from-7.2
Easy reference list of the main changes on PHP from 7.2 to 8.2.

Atualização 7.2 ao 8.2

Atualização 7.2 ao 8.2

PHP 7.2
- parse_str: informar segundo parâmetro para receber o resultado;
- number_format: primeiro parâmetro está tipado como float, precisa informar que é float
- object typehint: parametro object ou returno object, precisa ser objecto
- (*get_class not null)

PHP 7.3
- list: é capaz de tranformar um array em várias variáveis ordenadas, caso não exista a chave no array (ex, zip), uma variável por referencia pode ser usada, quando for adicionado a chave nova no array, a variavel da respectiva chave será setada.
```
$addr = ['Rua X', 54, 'Curitiba'];
list($address, $number, $city, &$state) = $addr; 

var_dump($address, $number, $city, $state); //'Rua X', 54, 'Curitiba', null

// Looping
$addr[3] = 'PR';
var_dump($address, $number, $city, $state); //'Rua X', 54, 'Curitiba', 'PR'
```

- compact: realiza um bind de uma string com uma variável de mesmo nome, é obrigatório todas as variáveis passadas existam.
```
$name = 'Lucas';
$email = 'lucas@teste.com.br';
$userCompact = compact('name','email');
var_dump($userCompact); // ['name' =- 'Lucas', 'email' =- 'lucas@teste.com.br']
```

- setcookie: alteração nos parametros, agora o terceiro parametro é um array de informações, permitindo informar apenas os dados necessarios
```
setcookie("CookieA", "HANDCLASS - PHP 7.2", time() + 3600, "", "", true);
setcookie("CookieB", "HANDCLASS - PHP 7.3", ['expires' =- time() + 3600, 'secure' =- true]);
```

- *json throw: verifica se o json é válido e dá um throw
- *array first & last key: pega o primeiro ou ultimo item do array
- *EOT
- *is_countable: verifica se uma variável é contável

PHP 7.4
- typed properties: possível tipar a propriedade, int, float, string, etc.
```
class Addr
{
   public string $addr;
}

class Client extends User
{
   public string $name;
   public int $age;
   public Addr $addr;
   public parent $user;
}

```
```
// Caso queira restringir a conversão automática de string para int (string "30" para int 30), adicionar no começo da aplicação:
declare(strict_types=1);
```
- arrow functions: na criação de funções anonimas, permite ocultar as chaves e o retorno, além de utilizar variáveis globais;
```
// Sem arrow function, função anonima pura
$firstName = function(string $fullName) : string {
   return explode(" ", $fullName)[0];
}
echo $firstName("Lucas Melo");

// Arrow function, opção 01
$fullName = "Lucas Melo";
$firstName = fn() => explode(" ", $fullName)[0];
echo $firstName(); // Lucas Melo

// Arrow function, opção 02
$firstName = fn($fullName) => explode(" ", $fullName)[0];
echo $firstName($fullName); // Lucas Melo
```

-*reference type: referencia da memória de uma variável, exemplo &$state;
-*underscore num separator: permite adicionar underscore em numeros inteiros no código para auxilia na visibilidade. 
```
echo 1_000_000; // 1000000
```
-*array inside parts: permite adicionar um array dentro de um array, adiciona três pontos no parâmetro, para permitir enviar várias variáveis na chamada da função.
```
// Using short array syntax.
// Also, works with array() syntax.
$arr1 = [1, 2, 3];
$arr2 = [...$arr1]; //[1, 2, 3]
$arr3 = [0, ...$arr1]; //[0, 1, 2, 3]
$arr4 = [...$arr1, ...$arr2, 111]; //[1, 2, 3, 1, 2, 3, 111]
$arr5 = [...$arr1, ...$arr1]; //[1, 2, 3, 1, 2, 3]

// Using short array syntax.
// Also, works with array() syntax.
$arr1 = [1, 2, 3];
$arr2 = [...$arr1]; //[1, 2, 3]
$arr3 = [0, ...$arr1]; //[0, 1, 2, 3]
$arr4 = [...$arr1, ...$arr2, 111]; //[1, 2, 3, 1, 2, 3, 111]
$arr5 = [...$arr1, ...$arr1]; //[1, 2, 3, 1, 2, 3]

public function testA($params, ...$params){}
```

PHP 8.0
- JIT: compilação Just-In-Time: O JIT compila e armazena em cache as instruções
PS.1 Para web não faz tanta diferença, o impacto maior é quanto uso de alto processamento
PS.2 Evite usar OPCACHE e JIT em localhost, o cache vai atrapalhar
```
// Enabling JIT in php.ini
opcache.enable=1
opcache.jit_buffer_size=100M
```

- constructor property promotion: não precisa mais declarar as propriedades de uma classe e não precisa associar as propriedades dentro do construtor, basta informar no construtor a tipagem
```
// Construtor padrão
class CarA
{
   public string $model;
   public int $year;
   
   public function __constuct(
      string $model,
      int $year   
   ) {
      $this->model = $model;
      $this->year = $year;
   }

   // Corpo da classe
}

// Usando construct property promotion
class CarB
{
   public function __constuct(
      public string $model,
      public int $year   
   ) {
      // Corpo do construtor
   }

   // Corpo da classe
}
```

- named params: funçoes, métodos e arrow functions recebem named params. Ao instanciar uma classe ou chamar uma função, é possível pular parâmetros não obrigatórios e informar apenas o parâmetro que deseja utilizar, adicionando o nome da variável:valor.
```
class CarC
{
   public function __constuct(
      public string $model,
      public string $brand = 'VM',
      public ?int $year   
   ) {
      // Corpo do construtor
   }

   // Corpo da classe
}

// Modo antigo
$car = new CarC('Polo', '', 2022);

// Usando named params
$car = new CarC('Polo', year:2022);

```


-*union and mixed types: é possível unir tipos, adicionando '|' na tipagem do dado com a respectiva tipagem desejada.
```
class CarD
{
   public function __constuct(
      public string $model,
      public int|null|object $year   
   ) {
      // Corpo do construtor
   }

   // Corpo da classe
}
```
-*match: substitui o switch. Ao contrário da switch, ela irá avaliar para um valor assim como as expressões ternárias. Diferente da switch, a comparação é uma verificação de identidade (===) em vez de uma comparação de equalidade fraca (==).
```
<?php
$comida = 'bolo';

$valor_de_retorno = match ($comida) {
    'apple' => 'Essa comida é uma maçã',
    'bar' => 'Essa comida é um bar',
    'bolo' => 'Essa comida é um bolo',
};

var_dump($valor_de_retorno);
?>
```
-*new functions: str_contains e outras funções.

PHP 8.1
- enums:
```
// Sem enums
class OrderTest
{
   private int $id;
   private int|float $amount;
   private int $status;

   public function createOrder(int $id, float $amount, int $status)
   {
      $this->id = $id;
      $this->amount = $amount;
      $this->status = $status;
      return $this;
   }
}

$orderTest = new OrderTest();
$orderTest->createOrder(22, 105.5, 1);
var_dump($orderTest);


// Com enums
enum PaymentStatus:int
{
   case approved = 1;
   case billed_printed = 2;
   case cancelled = 3;
   case chargeback = 4;
   case complete = 5;

   public function isPay(): bool
   {
      return match($this) {
         PaymentStatus::approved, Payment::complete => true,
         default => false
      }
   }
}

class OrderTest
{
   public function createOrder(
      private int $id, 
      private int|float $amount, 
      private PaymentStatus $status)
   {
   }

   public function getStatus(): PaymentStatus
   {
      return $this->status;
   }
}

$order = new Order(23, 105.5, PaymentStatus::approved);
var_dump(
   $order,
   $order->getStatus()->isPay(),
   [
      PaymentStatus::cases(),         // Lista todos os casos
      PaymentStatus::from(2),         // List o caso 2
      PaymentStatus::from(3)->name,   // Nome do status
      PaymentStatus::tryFrom(6),      //Traz o valor se existir ou null
      PaymentStatus::approved->value  // Traz o numero do status a partir do nome
   ]
);

```

- *fibers: forma de processar item um a um, ex: ao enviar uma fila de email, só vai executar um após o outro, ao inves de produzir grande memória e processamento, vai pausando o código.
-*readonly properties: propriedades somente leitura, como um ID que não pode ser modificado após criado no banco de dados;
-*intersection types: permite tipar uma variável usando regras complementares de classes combinadas, exemplo: string OU uma classe que implemente Countable e Stringable (exemplo abaixo).
```
function myCar(string|(Countable&Stringable) $model) {}
```
-*never return: tipo de retorno para fazer um método que faz um redirecionamento, terá um never ou exit.

PHP 8.2
- trait constant: muda o meio de configuração e setup
```
const DB_HOST = "localhost";
const DB_NAME = "fsphp";

// Problema é em acesso, qualquer um pode dar um vardump e ver o que está configurado
var_dump(DB_HOST); // "localhost"
var_dump(get_defined_constants(true)['user']); // Mostra todas as constantes desse usuário em um possível ataque da aplicação

// O ideal é não utilizar constante, apenas utilizar constante dentro das classes definidas que serão usadas
class DbConstants
{
   public function __construct(
      private string $host = DB_HOST,
      private string $name = DB_NAME
   ) {

   }
}

var_dump(new DbConstants());

// Usando Traits constant. Obs: trait é uma característica que só pode ser utilizado por uma classe que herde essa característica, assim, não é possível obter esses dados por acesso externo fora da classe instanciada.
trait DbConfig
{
   protected const HOST = "localhost",
   protected const NAME = "fsphp";
}

class Connect
{
   use DbConfig;
   public function __construct()
   {
      var_dump([self::HOST, self::NAME]);
   }
}
```
-(!) deprecations:
-- Dynamic Properties: criava e atribuía parametros de forma dinamica a uma classe, agora não é mais possível
``` 
class House
{
   public string $addr;
}
$house = new House();
$house->addr = "Rua X"; // A partir dessa versão, dará um erro, apesar de ser atribuído ao objeto
var_dump($house);       // "Rua X"

// Para permitir propriedades dinamicas, é necessário adicionar #[AllowDynamicProperties] no início da classe;
#[AllowDynamicProperties]
class House { ... }
```
-- UTF8 Encoding e Decoding: descontinuado, trocar por mb_convert_enconding
```
$string = "Ã à é ô"
var_dump([
   $string,
   utf_encode($string), // descontinuado
   utf_decode($string), // descontinuado
   $encode = mb_convert_enconding($string, 'UTF-8', 'ISO-8859-1'); // from iso to utf
   $decode = mb_convert_enconding($string, 'ISO-8859-1', 'UTF-8'); // from utf to iso
   mb_convert_enconding($encode, 'ISO-8859-1', 'UTF-8'); // from utf to iso
   mb_convert_enconding($decode, 'ISO-8859-1', 'UTF-8'); // from utf to iso
]);
```
-*disjoint normal form: única linguagem que implementa 100% ele, a partir do clean ode.
-*stand-alone types: consegue colocar true, false ou null.
-*readonly class: classe apenas leitura, exemplo classe API que nao pode mudar a URL, etc.
