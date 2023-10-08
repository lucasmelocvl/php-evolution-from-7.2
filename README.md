# php-evolution-from-7.2
Easy reference list of the main changes on PHP from 7.2 to 8.2.

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
