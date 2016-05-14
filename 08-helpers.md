----
## *Helpers* {#helpers}

Esta seção aborda os *Helpers* nativos do HXPHP.

----

### O que são *Helpers*? {#o-que-sao-helpers}

Os *Helpers* são componentes auxiliares. Em suma, são objetos que desempenham funções nas `views`.

----

### Alert Helper {#alert-helper}

O **Alert Helper** é um componente auxiliar que é responsável pela exibição de mensagens de aviso, erro, sucesso e afins.

Para utilizar este *helper* é necessário passar um *array* como parâmetro no construtor, que deve conter a seguinte estrutura:

```php
    array(
    	'danger', // Classes Bootstrap disponíveis: danger, warning, info, success
    	'Título da mensagem',
    	'Conteúdo da mensagem' // Pode ser um array com várias mensagens
    );
```


O código resultante seria:
```php
	<?php
	
    class ProdutosController extends \HXPHP\System\Controller
    {
  
      public function indexAction()
      {

      	$alerta = array(
      		'success',
      		'Uhuul! Produto cadastrado com sucesso!',
      		'O produto já está disponível para seus clientes.'
      	);

      	$this->load('Helpers\Alert', $alerta);

      	var_dump($this->alert->getAlert()); // ou
        echo $this->alert;
      }

	}
```


O método `getAlert()`, responsável pela renderização do alerta, é inserido na *view* e geralmente é encontrado em cabeçalhos para evitar a repetição de códigos.

É válido ressaltar que o alerta renderizado é gerado a partir de um template localizado na pasta: `src/HXPHP/System/Helpers/Alert/templates/`.

----
### Menu Helper {#menu-helper}

O **Menu Helper** tem a função de renderizar um menu customizado mediante o nível de acesso do usuário.

Diferentemente do *Alert Helper*, que trabalha com templates, este conta com um módulo específico de configuração. Portanto, antes de utilizá-lo, é necessário verificar se o módulo encontra-se registrado.

#### Recursos do módulo de configuração

O módulo contém dois métodos:
+ `setMenus()`: Que é responsável por definir a estrutura do menu e, opcionalmente, o nível de acesso;
+ `setConfigs()`: Que é responsável por customizar o menu que será renderizado.

##### Definindo a estrutura dos menus

Exemplo de configuração para definir dois menus (o primeiro para usuários com nível de acesso igual a **administrator** e o segundo que é neutro):

```php
    $configs->env->development->menu->setMenus(array(
      'Home/home' => '%siteURL%',
      'Projetos/briefcase' => '%baseURI%projetos/listar/',
      'Clientes/users' => array(
        'Listar todos/users' => '%baseURI%',
        'Tipos de clientes/users' => '%baseURI%clientes/tipos'
      ),
      'Usuários/users' => '%baseURI%usuarios/listar/'
    ), 'administrator');
    $configs->env->development->menu->setMenus(array(
      'Home/home' => '%siteURL%home',
      'Projetos/briefcase' => '%baseURI%projetos/listar/'
    ));
```
Arquivo: `app/config.php`


Este *helper* necessita de uma configuração em seu código para definir os níveis de acesso e seus respectivos menus e links. Para tal, acesse o arquivo `src/HXPHP/System/Helpers/Menu.php` e altere o conteúdo do método `setMenu()` mediante a sua necessidade.


Após configurar, injete o *Menu Helper* no controller e informe três argumentos para seu construtor: `O nível do usuário`, `o controller atual` e `a base dos links`.


Caso queira um menu genérico é só informar um valor qualquer no primeiro argumento. Para resgatar o controller atual utilize: `$this->request->controller` e para obter a baseURI: `$this->configs->baseURI`.


O código resultante seria:
```php
	<?php

    class ProdutosController extends \HXPHP\System\Controller
    {
      public function indexAction()
      {
        $this->load(
        	'Helpers\Menu',
        	null,
        	$this->request->controller,
        	$this->configs->baseURI
        );
      }

	}
```


Após 'setar' o menu utiliza-se o método `getMenu()` para renderizar o HTML, que pode ser configurado no código do próprio *helper*.


O código resultante seria:
```php
<?php echo $this->menu->getMenu(); ?>
```
