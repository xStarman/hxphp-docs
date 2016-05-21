----
<h2 id="modulos">*Módulos*</h2>

Nesta seção você irá conhecer os módulos.
----
<h3 id="o-que-sao-modulos">O que são Módulos?</h3>

Módulos são basicamente plugins, ou seja, são bibliotecas de códigos que tem o objetivo de adicionar recursos para os componentes do framework.
----
<h3 id="message-module">*Message Module*</h3>

O único módulo nativo do framework é o **Message Module**. Este módulo é responsável por gerenciar todas as mensagens de erro, aviso, sucesso e afins de todos os serviços do framework, sendo que também pode ser utilizado em outros componentes.

Para utilizar este módulo é necessário, antes de tudo, criar um template de mensagens no formato JSON. Este arquivo deve ser salvo na pasta `src/HXPHP/System/Modules/Messages/templates/`. Veja o exemplo do template `auth.json`:

```php
  {
    "description" : "Template de mensagens para o serviço de autenticação",
    "messages" : {
        "usuario-bloqueado" : {
            "subject" : "Acesso bloqueado por excesso de tentativas",
            "message" : "Ol&aacute; %s, <br /> Seu acesso foi bloqueado pelo excesso de tentativas, contate o administrador para liberação. Lembre-se de que esse recurso &eacute; para sua segurança, caso necess&aacute;rio solicite um novo acesso para evitar transtornos futuros."
        }
    },
    "alerts" : {
        "conta-em-uso" : {
            "style"   : "danger",
            "title"   : "Ops! Conta em uso!",
            "message" : "Esta conta j&aacute; est&aacute; registrada e sendo utilizada no momento!"
        },
        "tentativas-esgotando" : {
            "style"   : "warning",
            "title"   : "Ops! Verifique com cuidado os dados!",
            "message" : "Suas tentativas estão acabando, voc&ecirc; s&oacute; tem %s tentativa(s)! Ap&oacute;s exceder esse número seu acesso s&oacute; ser&aacute; liberado atrav&eacute;s da aprovação do administrador!"
        },
        "dados-incorretos" : {
            "style"   : "info",
            "title"   : "Ops! Dados incorretos!",
            "message" : "Não foi poss&iacute;vel realizar a autenticação, confira seus dados!"
        },
        "usuario-bloqueado" : {
            "style"   : "danger",
            "title"   : "Ops! Usu&aacute;rio bloqueado!",
            "message" : "Não foi poss&iacute;vel realizar a autenticação, pois este usu&aacute;rio encontra-se bloqueado no sistema, contate o administrador para liberação!"
        },
        "usuario-inexistente" : {
            "style"   : "danger",
            "title"   : "Ops! Dados incorretos!",
            "message" : "Não foi poss&iacute;vel realizar a autenticação, confira seus dados!"
        }
    }
  }

```

Nota-se que existem três estruturas principais:

+ Description;
+ Messages, e;
+ Alerts.

A estrutura **description** deve conter a descrição do template. Já a estrutura **messages** deve acomodar mensagens que serão enviadas por e-mail. E, por fim, a estrutura **alerts** deve acomodar as mensagens que serão renderizadas pelo <a href="#alert-helper">Alert Helper</a>.

As estruturas **messages** e **alerts** contém seções pré-definidas, que são elas:

+ **subject** e **message** para a a estrutura **messages**, e;
+ **style**, **title** e **message** para a estrutura **alerts**.

Estas seções devem ser acomodadas dentro de uma estrura intermediária que é nomeada com o código referente ao seu conteúdo.

Após criar e preencher os templates com seus respectivos conteúdos é possível resgatar estes valores de forma prática através do **Message Module**.

O primeiro passo é declarar para o método construtor do módulo qual é o nome do template que será utilizada, lembre-se de não informar a extensão .json, visto que a mesma é interpretada internamente.

O código resultante seria:
```php
  $messages = new Messages('auth');
  //ou
  $this->load('Modules\Messages', 'auth');
  ```

Para retornar o conteúdo de uma mensagem|alerta é necessário utilizar o seguinte caminho:

```php
  $this->messages->alerts->getByCode('conta-em-uso');
```

Executando o código acima tem-se como retorno um *array* no formato necessário para ser interpretado pelo <a href="#alert-helper">Alert Helper</a>.

Perceba que `{alerts}` é uma variável, ou seja, trata-se da estrutura que você deseja utilizar. No template anteriormente apresentado, pode-se observar duas estruturas: *messages* e *alerts*.

```php
  $this->messages->messages->getByCode('usuario-bloqueado');
```

Caso queira definir uma estrutura padrão e utilizar diretamente o método `getByCode($codigo)` configure o método `setBlock($estrutura);`.

```php
  $this->messages->setBlock('messages');
  $this->messages->getByCode('usuario-bloqueado');
```

Também é possível preencher valores coringas presentes na mensagem do template (%s), para tal, é necessário informar estes parâmetros em um *array* seguindo o padrão abaixo:

Template para exemplo:
```php
  {
    "description" : "Template de mensagens para exemplo",
    "alerts" : {
        "codigo" : {
            "style"   : "info",
            "title"   : "%s",
            "message" : "Olá %s %s, como vai?"
        }
    }
  }
```
O código resultante seria:
```php
  $this->load('Modules\Messages', 'meutemplate');
  $this->messages->setBlock('alerts');

  $substituir_coringas = array(
    'title' => 'Hello World',
    'message' => array(
      'Bruno',
      'Santos'
    )
  );


  $this->messages->getByCode('codigo', $substituir_coringas);
```