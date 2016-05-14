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

Diferentemente do *Alert Helper*, que trabalha com templates, este conta com um módulo específico de configuração. Portanto, antes de utilizá-lo, é necessário verificar se o módulo está registrado.

#### Recursos do módulo de configuração

O módulo contém dois métodos:
+ `setMenus()`: Que é responsável por definir a estrutura do menu e, opcionalmente, o nível de acesso;
+ `setConfigs()`: Que é responsável por customizar o menu que será renderizado.

##### Definindo a estrutura dos menus

Exemplo de configuração para definir dois menus (o primeiro para usuários com nível de acesso igual a **administrator** e o segundo que é neutro):

```php
    $configs->env->development->menu->setMenus(array(
      'Home/home' => '%siteURL%',
      'Projetos/briefcase' => '%baseURI/%projetos/listar/',
      'Clientes/users' => array(
        'Listar todos/users' => '%baseURI%',
        'Tipos de clientes/users' => '%baseURI/%clientes/tipos'
      ),
      'Usuários/users' => '%baseURI%/usuarios/listar/'
    ), 'administrator');


    $configs->env->development->menu->setMenus(array(
      'Home/home' => '%siteURL%/home',
      'Projetos/briefcase' => '%baseURI%/projetos/listar/'
    ));
```

Observações:
+ O método suporta 2 argumentos: *(array)* $menus e *(string)* $access_level;
+ O segundo argumento é opcional, isto é, também é possível definir um menu sem um nível de acesso para páginas públicas e afins;
+ O *array* com os menus é padronizado da seguinte forma:
```php
array(
  'Menu Sem Dropdown/codigo-icone-font-awesome-sem-prefixo' => 'http://www.link-absoluto.com',
  'Usuários/users' => '%baseURI%/link-relativo',
  'Menu Com Dropdown/home' => array(
    'Submenu/briefcase' => '%baseURI%/link-relativo',
    'Submenu #2/users' => '%baseURI%/link-relativo'
  )
)
```
+ É possível utilizar os coringas `%baseURI%` e `%siteURL%` que são substituídos pelo valor da configuração `baseURI` e pelo endereço do site, respectivamente;
+ A ferramenta [Font Awesome](http://fontawesome.io/) foi utilizada nos ícones que acompanham os títulos nos menus e submenus, e;
+ O prefixo `fa` utilizado para a exibição dos ícones é inserido nativamente.

##### Customizando o menu

Exemplo com todas as possíveis configurações do menu:
```php
    $configs->env->development->menu->setMenus(array(
      'container' => false, // Tag container (opcional). Ex: nav
      'container_id' => '',
      'container_class' => '',
      'menu_id' => 'menu',
      'menu_class' => 'menu',
      'menu_item_class' => 'menu-item',
      'menu_item_active_class' => 'active',
      'menu_item_dropdown_class' => 'dropdown',
      'link_before' => '<span>', // Conteúdo anterior ao título
      'link_after' => '</span>', // Conteúdo posterior ao título
      'link_class' => 'menu-link',
      'link_active_class' => 'menu-active-link',
      'link_dropdown_class' => 'dropdown-toggle',
      'link_dropdown_attrs' => array(
        'data-toggle' => 'dropdown' // Data Atributos para ativação do dropdown
      ),
      'dropdown_class' => 'dropdown-menu',
      'dropdown_item_class' => 'dropdown-item',
      'dropdown_item_active_class' => 'active'
    ));
```

O exemplo acima está com todos os valores padrão, portanto, caso necessite customizar o seu menu, insira **apenas as configurações que deseja alterar** o valor padrão.

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
