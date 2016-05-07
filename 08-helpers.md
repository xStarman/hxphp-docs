<hr class="col-md-12">
<h2 id="helpers"><em>Helpers</em></h2>
Esta seção aborda os <em>Helpers</em>.
<hr class="col-md-12">
<h3 id="o-que-sao-helpers">O que são <em>Helpers</em>?</h3>
Os <em>Helpers</em> são componentes auxiliares. Na verdade, os <em>Helpers</em> são criados com o objetivo de armazenar funções comuns que podem ser utilizadas nas <em>views</em>.
<hr class="col-md-12">
<h3 id="alert-helper">Alert Helper</h3>
Um dos <em>helpers</em> nativos do HXPHP Framework é o <strong>Alert Helper</strong> que trata-se de um componente auxiliar para a exibição de mensagens de aviso, erro, sucesso e afins.

Para utilizar este <em>helper</em> é necessário passar um <em>array</em> como parâmetro no construtor. Este deve conter a seguinte estrutura:

<?=syntaxHighlight('
    array(
    	\'danger\', // Classes Bootstrap disponíveis: danger, warning, info, success
    	\'Título da mensagem\',
    	\'Conteúdo da mensagem\' // Pode ser um array com várias mensagens
    );
');?>


<small>O código resultante seria:</small>
<?=syntaxHighlight('
	<?php
	
    class ProdutosController extends \HXPHP\System\Controller
    {
  
      public function indexAction()
      {

      	$alerta = array(
      		\'success\',
      		\'Uhuul! Produto cadastrado com sucesso!\',
      		\'O produto j&aacute; est&aacute; dispon&iacute;vel para seus clientes.\'
      	);

      	$this->load(\'Helpers\Alert\', $alerta);

      	var_dump($this->alert->getAlert());
      }

	}
');?>


O método <code>getAlert()</code> é inserido na <em>view</em> e geralmente é encontrado em cabeçalhos para evitar a repetição de códigos.


É válido ressaltar que o template HTML renderizado pode ser editado. Os arquivos encontram-se na pasta <code>src/HXPHP/System/Helpers/templates/Alert/</code>.

<hr class="col-md-12">
<h3 id="menu-helper">Menu Helper</h3>

Outro <em>helper</em> nativo é o <strong>Menu Helper</strong> que tem a função de renderizar um menu customizado mediante o nível de acesso do usuário.


Este <em>helper</em> necessita de uma configuração em seu código para definir os níveis de acesso e seus respectivos menus e links. Para tal, acesse o arquivo <code>src/HXPHP/System/Helpers/Menu.php</code> e altere o conteúdo do método <code>setMenu()</code> mediante a sua necessidade.


Após configurar, injete o <em>Menu Helper</em> no controller e informe três argumentos para seu construtor: <code>O nível do usuário</code>, <code>o controller atual</code> e <code>a base dos links</code>.


Caso queira um menu genérico é só informar um valor qualquer no primeiro argumento. Para resgatar o controller atual utilize: <code>$this->request->controller</code> e para obter a baseURI: <code>$this->configs->baseURI</code>.


<small>O código resultante seria:</small>
<?=syntaxHighlight('
	<?php

    class ProdutosController extends \HXPHP\System\Controller
    {
      public function indexAction()
      {
        $this->load(
        	\'Helpers\Menu\',
        	null,
        	$this->request->controller,
        	$this->configs->baseURI
        );
      }

	}
');?>


Após 'setar' o menu utiliza-se o método <code>getMenu()</code> para renderizar o HTML, que pode ser configurado no código do próprio <em>helper</em>.


<small>O código resultante seria:</small>
<?=syntaxHighlight('<?php echo $this->menu->getMenu(); ?>');?>
