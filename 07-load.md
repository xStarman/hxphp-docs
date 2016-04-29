	<hr class="col-md-12">
	<h2 id="metodo-load">Método <em>load()</em></h2>
	<p>
		Esta seção aborda o carregamento de <em>Helpers</em>, Serviços, Módulos e afins nos <em>Controllers</em>.
	</p>
	<hr class="col-md-12">
	<p>
		Para utilizar os <em>helpers</em> e afins tanto na view como no controller é necessário "injetar" o objeto no controller através do método <code>load()</code>. Este método contém apenas um argumento obrigatório que é a <strong>localização do objeto</strong>. É necessário informar a <em>namespace</em> correspondente e o nome do objeto. A <em>namespace</em>, em suma, é o nome da pasta em que o objeto se encontra.
	</p>
	<p>
		Os demais argumentos informados no método <code>load('Helpers\NomeDoHelper', $param1, $param2, $paramN)</code> serão utilizados como argumentos no método construtor do objeto que será carregado.
	</p>
	<p>
		<small>O código resultante seria:</small>
		<?=syntaxHighlight('
			<?php

	        class ProdutosController extends \HXPHP\System\Controller
	        {
          
	          public function indexAction()
	          {
	          	$this->load(\'Helpers\NomeDoHelper\', \'param1\', \'param2\');
	          	$this->load(\'Modules\NomeDoModulo\', \'param1\', \'param2\');
	          	$this->load(\'Storage\Session\');
	          	$this->load(\'Services\Email\');

	          	$this->nomedohelper->metodo();
	          	echo $this->nomedomodulo->propriedade;
	          	...
	          }

        	}
	    ');?>
	</p>
	<p>
		Após injetar o objeto, no controller, cria-se um atributo com o seu nome no padrão <em>lowercase</em>.
	</p>