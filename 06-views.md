----
## *Views* {#views}

Nesta seção você irá conhecer mais uma das três camadas do padrão **MVC**.

----
### O que são *views*? {#o-que-sao-views}

As *views*, comumente chamadas de **interfaces**, são responsáveis por toda parte gráfica de nossa aplicação. É a camada que acomoda nossos códigos HTML.

----
### Criando *views* {#criando-views}

Para criar uma *view* é necessário ter um cuidado fundamental que é: verificar a extensão definida para salvar o arquivo. Portanto, verifique o valor da configuração global `$configs->global->views->extension`.

Por padrão a extensão das *views* é a `.phtml`. Ciente da extensão necessária é só salvar a *view* na pasta responsável.

As *views* são organizadas da seguinte forma: <br>
*app/views/`{nomedocontroller}`/`{nomedaaction}`.`phtml`*.

#### Subpastas

Quando o parâmetro `subfolder` é existente, isto é, atende aos requisitos mencionados na seção de Funcionamento da URL, as *views* também deverão ser organizadas nas respectivas subpastas do diretório `app/views`. 

Um exemplo para o link (`http://www.site.com.br/hxphp/admin/produtos/listar`):

```
app/
	views/
		admin/
			produtos/
				listar.phtml
```

#### Convenções

O padrão é *lowercase*. Todas as *views* de um *controller* são armazenadas em uma pasta com o nome do *controller*. Cada view é nomeada de acordo com a *action*. Porém, é possível alterar estas configurações através do objeto **View**.

----
### Configurando a *View* {#configurando-a-view}

Estas são as opções disponíveis para configurar uma view:

+ `setPath($string)` - Define a pasta do arquivo - *Padrão: nome do controller*
+ `setFile($string)` - Define o nome do arquivo - *Padrão: nome da action*
+ `setHeader($string)` - Define o cabeçalho da view - *Padrão: header*
+ `setFooter($string)` - Define o rodapé da view - *Padrão: footer*
+ `setTemplate($boolean)` - Define se o cabeçalho e rodapé serão inclusos - *Padrão: true*
+ `setAssets($string, $string | $array)` - Define arquivos CSS ou JS customizados para a view - *O primeiro parâmetro determina o tipo de arquivo, isto é, css ou js*
+ `setTitle($string)` - Define o título da view - *A variável $view_title recebe o valor desta configuração*

*O código resultante seria:*
```  {.brush:php}
	<?php

    class ProdutosController extends \HXPHP\System\Controller
    {

      public function indexAction()
      {
       	$this->view->setPath('index')
			   ->setFile('index')
			   ->setHeader('header')
			   ->setFooter('footer')
			   ->setTemplate(true)
			   ->setAssets('css', 'teste.css')
			   ->setAssets('css', array(
			   		'teste2.css',
			   		'teste3.css'
			   	))
			   ->setAssets('js', array(
			   		'teste2.js',
			   		'teste3.js'
			   	))
			   ->setTitle('Oops! Nada encontrado!');
      }

	}
```
----
### Enviando dados para a *View* {#enviando-dados-para-a-view}

```  {.brush:php}
	<?php

    class ProdutosController extends \HXPHP\System\Controller
    {

      public function indexAction()
      {
       	$this->view->setVar('mensagem', 'Hello World')
			   ->setVars(array(
			   		'usuario' => 'Bruno Santos',
			   		'facebook' => 'https://www.facebook.com/brunocsantos2012'
			   	));
      }

	}
```

Na *view* todos estes dados são extraídos e atribuídos em variáveis com o sufixo `view_`. No exemplo acima, as variáveis disponíveis na view seriam:

+ $view_mensagem
+ $view_usuario
+ $view_facebook