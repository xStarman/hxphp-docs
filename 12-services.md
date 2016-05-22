----
## Serviços {#servicos}

Nesta seção você irá conhecer os serviços nativos do HXPHP Framework.

----
### O que são Serviços? {#o-que-sao-servicos}

Serviços são objetos que executam uma ação específica, ou seja, o serviço de autenticação tem como objetivo autenticar o usuário e assim por diante. Os serviços do HXPHP Framework são armazenados na pasta `src/HXPHP/System/Services/`.

----
### Serviço de Autenticação {#servicos-lista}

Um dos serviços do HXPHP Framework é o responsável pela autenticação. Ele contém os seguintes métodos:

+ `login($user_id, $username)` - Autentica o usuário;
+ `logout()` - Exclui sessões de autenticação;
+ `redirectCheck($public_page)` - Redireciona o usuário;
+ `login_check()` - Verifica se o usuário está autenticado, e;
+ `getUserId()` - Retorna o Id do usuário autenticado.

Este serviço conta com 3 parâmetros de configuração no método construtor. Sendo eles, respectivamente:

+ URL de redirecionamento após o login bem sucedido;
+ URL de redirecionamento após o logout, e;
+ Booleano que determina se o redirecionamento será automático no método `login(...);`.

Para facilitar a configuração deste serviço existe um módulo de configuração. Portanto, em vez de setar os valores repetidamente em todos os carregamentos do serviço, pode-se centralizar no arquivo de configuração `app/config.php`.

Configuração na prática:
```php
      <?php
        // app/config.php
      	...
      	$configs->env->development->auth->setURLs('/sistema/home/', '/sistema/login/');
      	...
```
	


  Serviço na prática:
```php
      <?php
        class LoginController extends \HXPHP\System\Controller
        {
      
            public function __construct($configs)
            {
            	parent::__construct($configs);

                $this->load(
					'Services\Auth',
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
				$this->auth->login(1, 'brunosantos');
			}

			public function sairAction()
			{
				$this->auth->logout();
			}
        }
```


Este serviço geralmente trabalha em conjunto com o *model* de usuários e autentica somente após todas as validações obterem sucesso.

----
### Serviço de E-mail {#servico-de-e-mail}

O serviço de e-mail é um dos mais simples presentes no HXPHP Framework, ele contém apenas um método `send()` que necessita dos seguintes parâmetros:

+ E-mail para qual será enviada a mensagem;
+ Assunto da mensagem;
+ Mensagem, e;
+ Array com Remetente e E-mail do remetente.

Após executar o método `send()` ele retornará um booleano com o status do processo.


Serviço na prática:
```php
      <?php
        class ProdutosController extends \HXPHP\System\Controller
        {

            public function comprarAction()
            {
            $this->load('Services\Email');
            $status = $this->email->send('fulano@email.com.br', 'Compra realizada com sucesso!', 'Mensagem', array(
            	'remetente' => $this->configs->mail->from,
            	'email' => $this->configs->mail->from_mail
            ));

            }

        }
```

----
### Serviço de Redefinição de Senhas {#servico-de-redefinicao-de-senhas}

O serviço de redefinição de senhas também é extremamente simples e conta com apenas três métodos:

+ `setLink($link)` - Define o link;
+ `sendRecoveryLink($name, $email)` - Envia a mensagem de redefinição de senha, e;
+ `getMessage()` - Retorna as mensagens do template **password-recovery.json**.

Serviço na prática:
```php
    <?php
        class EsqueciASenhaController extends \HXPHP\System\Controller
        {
            public function enviarAction()
            {
            $this->load('PasswordRecovery');
			$this->passwordrecovery->setLink(SITE . 'esqueci-a-senha/redefinir/');

			$user = User::find_by_username($this->request->post('username'));
			$callback_message = $this->passwordrecovery->sendRecoveryLink($user->full_name, $user->email);

            }

        }
```


O serviço de redefinição de senha além de enviar o link disponibiliza o token gerado para ser armazenado no banco de dados para validação futura e conclusão do processo.


Obtenção do TOKEN:
```php
    $token = $this->passwordrecovery->token;
```

----
### Serviço de Sessão {#servico-de-sessao}

O serviço de sessão tem a única finalidade de iniciar a sessão do PHP de forma personalizada, para tal, utiliza-se o método estático `sec_session_start()`.


  Serviço na prática:
```php
	\HXPHP\System\Services\StartSession::sec_session_start();
```
