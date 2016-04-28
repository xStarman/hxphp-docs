<hr class="col-md-12">
<h2 id="views"><em>Views</em></h2>
<p>
Nesta seção você irá conhecer mais uma das três camadas do padrão <strong>MVC</strong>.
</p>
<hr class="col-md-12">
<h3 id="o-que-sao-views">O que são <em>views</em>?</h3>
<p>
As <em>views</em>, comumente chamadas de <strong>interfaces</strong>, são responsáveis por toda parte gráfica de nossa aplicação. É a camada que acomoda nossos códigos HTML.
</p>
<hr class="col-md-12">
<h3 id="criando-views">Criando <em>views</em></h3>
<p>
Para criar uma <em>view</em> é necessário ter um cuidado fundamental que é verificar a extensão a qual deverá ser utilizada para salvar o arquivo. Portanto, verifique o valor da configuração global <code>$configs->global->views->extension</code>.
</p>
<p>
Por padrão a extensão das <em>views</em> é a <code>.phtml</code>. Ciente da extensão necessária é só salvar a <em>view</em> na pasta responsável.
</p>
<p>
As <em>views</em> são organizadas da seguinte forma: <br>
<em>app/views/<code>{nomedocontroller}</code>/<code>{nomedaaction}</code>.<code>phtml</code></em>.
</p>
<p>
O padrão é <em>lowercase</em>. Todas as <em>views</em> de um <em>controller</em> são armazenadas em uma pasta com o nome do <em>controller</em>. Cada view é nomeada de acordo com a <em>action</em>. Porém, é possível alterar estas configurações através do objeto <strong>View</strong>.
</p>
<hr class="col-md-12">
<h3 id="configurando-a-view">Configurando a <em>View</em></h3>
<p>
Estas são as opções disponíveis para configurar uma view:
<ul>
	<li>
		<code>setPath($string)</code> - Define a pasta do arquivo - <small>Padrão: nome do controller</small>
	</li>
	<li>
		<code>setFile($string)</code> - Define o nome do arquivo - <small>Padrão: nome da action</small>
	</li>
	<li>
		<code>setHeader($string)</code> - Define o cabeçalho da view - <small>Padrão: header</small>
	</li>
	<li>
		<code>setFooter($string)</code> - Define o rodapé da view - <small>Padrão: footer</small>
	</li>
	<li>
		<code>setTemplate($boolean)</code> - Define se o cabeçalho e rodapé serão inclusos - <small>Padrão: <em>true</em></small>
	</li>
	<li>
		<code>setAssets($string, $string | $array)</code> - Define arquivos CSS ou JS customizados para a view
	</li>
	<li>
		<code>setTitle($string)</code> - Define o título da view
	</li>
</ul>
</p>
<p>
<small>O código resultante seria:</small>
<?=syntaxHighlight('
	<?php

    class ProdutosController extends \HXPHP\System\Controller
    {

      public function indexAction()
      {
       	$this->view->setPath(\'index\')
			   ->setFile(\'index\')
			   ->setHeader(\'header\')
			   ->setFooter(\'footer\')
			   ->setTemplate(true)
			   ->setAssets(\'css\', \'teste.css\')
			   ->setAssets(\'css\', array(
			   		\'teste2.css\',
			   		\'teste3.css\'
			   	))
			   ->setAssets(\'js\', array(
			   		\'teste2.js\',
			   		\'teste3.js\'
			   	))
			   ->setTitle(\'Oops! Nada encontrado!\');
      }

	}
');?>
</p>
<hr class="col-md-12">
<h3 id="enviando-dados-para-a-view">Enviando dados para a <em>View</em></h3>
<p>
<?=syntaxHighlight('
	<?php

    class ProdutosController extends \HXPHP\System\Controller
    {

      public function indexAction()
      {
       	$this->view->setVar(\'mensagem\', \'Hello World\')
			   ->setVars(array(
			   		\'usuario\' => \'Bruno Santos\',
			   		\'facebook\' => \'https://www.facebook.com/brunocsantos2012\'
			   	));
      }

	}
');?>
</p>
<p>
Na <em>view</em> todos estes dados são extraídos e atribuídos em variáveis com o sufixo <code>view_</code>. No exemplo acima, as variáveis disponíveis na view seriam:
<ul>
	<li>$view_mensagem</li>
	<li>$view_usuario</li>
	<li>$view_facebook</li>
</ul>
</p>