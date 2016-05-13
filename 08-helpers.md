----
<h2 id="helpers">*Helpers*</h2>
Esta seção aborda os *Helpers*.
----
<h3 id="o-que-sao-helpers">O que são *Helpers*?</h3>
Os *Helpers* são componentes auxiliares. Na verdade, os *Helpers* são criados com o objetivo de armazenar funções comuns que podem ser utilizadas nas *views*.
----
<h3 id="alert-helper">Alert Helper</h3>
Um dos *helpers* nativos do HXPHP Framework é o **Alert Helper** que trata-se de um componente auxiliar para a exibição de mensagens de aviso, erro, sucesso e afins.

Para utilizar este *helper* é necessário passar um *array* como parâmetro no construtor. Este deve conter a seguinte estrutura:

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
      		'O produto j&aacute; est&aacute; dispon&iacute;vel para seus clientes.'
      	);

      	$this->load('Helpers\Alert', $alerta);

      	var_dump($this->alert->getAlert());
      }

	}
```


O método `getAlert()` é inserido na *view* e geralmente é encontrado em cabeçalhos para evitar a repetição de códigos.


É válido ressaltar que o template HTML renderizado pode ser editado. Os arquivos encontram-se na pasta `src/HXPHP/System/Helpers/templates/Alert/`.

----
<h3 id="menu-helper">Menu Helper</h3>

Outro *helper* nativo é o **Menu Helper** que tem a função de renderizar um menu customizado mediante o nível de acesso do usuário.


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
