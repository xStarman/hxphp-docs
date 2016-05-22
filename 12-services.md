	<hr class="col-md-12">
	<h2 id="servicos">Serviços</h2>
	<p>
		Nesta seção você ir&aacute; conhecer os serviços nativos do HXPHP Framework.
	</p>
	<hr class="col-md-12">
	<h3 id="o-que-sao-servicos">O que são Serviços?</h3>
	<p>
		Serviços são objetos que executam uma ação específica, ou seja, o serviço de autenticação tem como objetivo autenticar o usuário e assim por diante. Os serviços do HXPHP Framework são armazenados na pasta <code>src/HXPHP/System/Services/</code>.
	</p>
	<hr class="col-md-12" id="servicos-lista">
	<h3 id="servico-de-autenticacao">Serviço de Autenticação</h3>
	<p>
		Um dos serviços do HXPHP Framework é o responsável pela autenticação. Ele contém os seguintes métodos:
		<ul>
			<li><code>login($user_id, $username)</code> - Autentica o usuário;</li>
			<li><code>logout()</code> - Exclui sessões de autenticação;</li>
			<li><code>redirectCheck($public_page)</code> - Redireciona o usuário;</li>
			<li><code>login_check()</code> - Verifica se o usuário está autenticado, e;</li>
			<li><code>getUserId()</code> - Retorna o Id do usuário autenticado.</li>
		</ul>
	</p>
	<p>
		Este serviço conta com 3 parâmetros de configuração no método construtor. Sendo eles, respectivamente:
		<ul>
			<li>URL de redirecionamento após o login bem sucedido;</li>
			<li>URL de redirecionamento após o logout, e;</li>
			<li>Booleano que determina se o redirecionamento será automático no método <code>login(...);</code>.</li>
		</ul>
	</p>
	<p>
		Para facilitar a configuração deste serviço existe um módulo de configuração. Portanto, em vez de setar os valores repetidamente em todos os carregamentos do serviço, pode-se centralizar no arquivo de configuração <code>app/config.php</code>.
		<br>
		<small>Configuração na prática:</small>
		  <?=syntaxHighlight('
		      <?php
		        // app/config.php
		      	...
		      	$configs->env->development->auth->setURLs(\'/sistema/home/\', \'/sistema/login/\');
		      	...
		   ');?>
		</p>	
	</p>
	<p>
	  <small>Serviço na prática:</small>
	  <?=syntaxHighlight('
	      <?php
	        class LoginController extends \HXPHP\System\Controller
	        {
	      
	            public function __construct($configs)
	            {
	            	parent::__construct($configs);

	                $this->load(
						\'Services\Auth\',
						$configs->auth->after_login,
						$configs->auth->after_logout,
						true
					);
					
					// Páginas públicas
					$this->auth->redirectCheck(true);

					// Páginas privadas
					//$this->auth->redirectCheck();
	            }


				public function logarAction()
				{
					$this->auth->login(1, \'brunosantos\');
				}

				public function sairAction()
				{
					$this->auth->logout();
				}
	        }
	   ');?>
	</p>
	<p>
		Este serviço geralmente trabalha em conjunto com o <em>model</em> de usuários e autentica somente após todas as validações obterem sucesso.
	</p>
	<hr class="col-md-12">
	<h3 id="servico-de-e-mail">Serviço de E-mail</h3>
	<p>
		O serviço de e-mail é um dos mais simples presentes no HXPHP Framework, ele contém apenas um método <code>send()</code> que necessita dos seguintes parâmetros:
		<ul>
			<li>E-mail para qual será enviada a mensagem;</li>
			<li>Assunto da mensagem;</li>
			<li>Mensagem, e;</li>
			<li>Array com Remetente e E-mail do remetente.</li>
		</ul>
		Após executar o método <code>send()</code> ele retornará um booleano com o status do processo.
	</p>
	<p>
	  <small>Serviço na prática:</small>
	  <?=syntaxHighlight('
	      <?php
	        class ProdutosController extends \HXPHP\System\Controller
	        {

	            public function comprarAction()
	            {
	            	$this->load(\'Services\Email\');
	            	$status = $this->email->send(\'fulano@email.com.br\', \'Compra realizada com sucesso!\', \'Mensagem\', array(
	            		\'remetente\' => $this->configs->mail->from,
	            		\'email\' => $this->configs->mail->from_mail
	            	));

	            }

	        }
	   ');?>
	</p>
	<!--<hr class="col-md-12">
	<h3 id="servico-de-redefinicao-de-senhas">Serviço de Redefinição de Senhas</h3>
	<p>
		O serviço de redefinição de senhas também é extremamente simples e conta com apenas três métodos:
		<ul>
			<li><code>setLink($link)</code> - Define o link;</li>
			<li><code>sendRecoveryLink($name, $email)</code> - Envia a mensagem de redefinição de senha, e;</li>
			<li><code>getMessage()</code> - Retorna as mensagens do template <strong>password-recovery.json</strong>.</li>
		</ul>
	</p>
	<p>
	  <small>Serviço na prática:</small>
	  <?=syntaxHighlight('
	      <?php
	        class EsqueciASenhaController extends \HXPHP\System\Controller
	        {

	            public function indexAction()
	            {
	            }

	            public function enviarAction()
	            {
	            	$this->load(\'PasswordRecovery\');
					$this->passwordrecovery->setLink(SITE.\'esqueci-a-senha/redefinir/\');

					$user = User::find_by_username($this->request->post(\'username\'));
					$callback_message = $this->passwordrecovery->sendRecoveryLink($user->full_name, $user->email);

	            }

	        }
	   ');?>
	</p>
	<p>
		O serviço de redefinição de senha além de enviar o link disponibiliza o token gerado para ser armazenado no banco de dados para validação futura e conclusão do processo.
	</p>
	<p>
	  <small>Obtenção do TOKEN:</small>
	  <?=syntaxHighlight('
	      $token = $this->passwordrecovery->token; ');?>
	</p>-->
	<hr class="col-md-12">
	<h3 id="servico-de-sessao">Serviço de Sessão</h3>
	<p>
		O serviço de sessão tem a única finalidade de iniciar a sessão do PHP de forma personalizada, para tal, utiliza-se o método estático <code>sec_session_start()</code>.
	</p>
	<p>
	  <small>Serviço na prática:</small>
	  <?=syntaxHighlight('\HXPHP\System\Services\StartSession::sec_session_start(); ');?>
	</p>