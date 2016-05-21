    <hr class="col-md-12">
    <h2 id="modulos"><em>Módulos</em></h2>
    <p>
        Nesta seção você ir&aacute; conhecer os módulos.
    </p>
    <hr class="col-md-12">
    <h3 id="o-que-sao-modulos">O que são Módulos?</h3>
    <p>
        Módulos são basicamente plugins, ou seja, são bibliotecas de códigos que tem o objetivo de adicionar recursos para os componentes do framework.
    </p>
    <hr class="col-md-12">
    <h3 id="message-module"><em>Message Module</em></h3>
    <p>
        O único módulo nativo do framework é o <strong>Message Module</strong>. Este módulo é responsável por gerenciar todas as mensagens de erro, aviso, sucesso e afins de todos os serviços do framework, sendo que também pode ser utilizado em outros componentes.
    </p>
    <p>
        Para utilizar este módulo é necessário, antes de tudo, criar um template de mensagens no formato JSON. Este arquivo deve ser salvo na pasta <code>src/HXPHP/System/Modules/Messages/templates/</code>. Veja o exemplo do template <code>auth.json</code>:
    </p>
    <p>
    <?=syntaxHighlight('
      {
        "description" : "Template de mensagens para o servi&ccedil;o de autentica&ccedil;&atilde;o",
        "messages" : {
            "usuario-bloqueado" : {
                "subject" : "Acesso bloqueado por excesso de tentativas",
                "message" : "Ol&aacute; %s, <br /> Seu acesso foi bloqueado pelo excesso de tentativas, contate o administrador para libera&ccedil;&atilde;o. Lembre-se de que esse recurso &eacute; para sua seguran&ccedil;a, caso necess&aacute;rio solicite um novo acesso para evitar transtornos futuros."
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
                "message" : "Suas tentativas est&atilde;o acabando, voc&ecirc; s&oacute; tem %s tentativa(s)! Ap&oacute;s exceder esse número seu acesso s&oacute; ser&aacute; liberado atrav&eacute;s da aprova&ccedil;&atilde;o do administrador!"
            },
            "dados-incorretos" : {
                "style"   : "info",
                "title"   : "Ops! Dados incorretos!",
                "message" : "N&atilde;o foi poss&iacute;vel realizar a autentica&ccedil;&atilde;o, confira seus dados!"
            },
            "usuario-bloqueado" : {
                "style"   : "danger",
                "title"   : "Ops! Usu&aacute;rio bloqueado!",
                "message" : "N&atilde;o foi poss&iacute;vel realizar a autentica&ccedil;&atilde;o, pois este usu&aacute;rio encontra-se bloqueado no sistema, contate o administrador para libera&ccedil;&atilde;o!"
            },
            "usuario-inexistente" : {
                "style"   : "danger",
                "title"   : "Ops! Dados incorretos!",
                "message" : "N&atilde;o foi poss&iacute;vel realizar a autentica&ccedil;&atilde;o, confira seus dados!"
            }
        }
      }

    ');?>
  </p>
  <p>
    Nota-se que existem três estruturas principais:
    <ul>
        <li>Description;</li>
        <li>Messages, e;</li>
        <li>Alerts.</li>
    </ul>
  </p>
  <p>
    A estrutura <strong>description</strong> deve conter a descrição do template. Já a estrutura <strong>messages</strong> deve acomodar mensagens que serão enviadas por e-mail. E, por fim, a estrutura <strong>alerts</strong> deve acomodar as mensagens que serão renderizadas pelo <a href="#alert-helper">Alert Helper</a>.
  </p>
  <p>
    As estruturas <strong>messages</strong> e <strong>alerts</strong> contém seções pré-definidas, que são elas:
    <ul>
        <li><strong>subject</strong> e <strong>message</strong> para a a estrutura <strong>messages</strong>, e;</li>
        <li><strong>style</strong>, <strong>title</strong> e <strong>message</strong> para a estrutura <strong>alerts</strong>.</li>
    </ul>
  </p>
  <p>
    Estas seções devem ser acomodadas dentro de uma estrura intermediária que é nomeada com o código referente ao seu conteúdo.
  </p>
  <p>
    Após criar e preencher os templates com seus respectivos conteúdos é possível resgatar estes valores de forma prática através do <strong>Message Module</strong>.
  </p>
  <p>
    O primeiro passo é declarar para o método construtor do módulo qual é o nome do template que será utilizada, lembre-se de não informar a extensão .json, visto que a mesma é interpretada internamente.
  </p>
  <p>
    <small>O código resultante seria:</small>
    <?=syntaxHighlight('
      $messages = new Messages(\'auth\');
      //ou
      $this->load(\'Modules\Messages\', \'auth\');
      ');?>
  </p>
  <p>
    Para retornar o conteúdo de uma mensagem|alerta é necessário utilizar o seguinte caminho:
  </p>
  <p>
    <?=syntaxHighlight('
      $this->messages->alerts->getByCode(\'conta-em-uso\');');?>
  </p>
  <p>
    Executando o código acima tem-se como retorno um <em>array</em> no formato necessário para ser interpretado pelo <a href="#alert-helper">Alert Helper</a>.
  </p>
  <p>
    Perceba que <code>{alerts}</code> é uma variável, ou seja, trata-se da estrutura que você deseja utilizar. No template anteriormente apresentado, pode-se observar duas estruturas: <em>messages</em> e <em>alerts</em>.
  </p>
  <p>
    <?=syntaxHighlight('
      $this->messages->messages->getByCode(\'usuario-bloqueado\');');?>
  </p>
  <p>
    Caso queira definir uma estrutura padrão e utilizar diretamente o método <code>getByCode($codigo)</code> configure o método <code>setBlock($estrutura);</code>.
  </p>
  <p>
    <?=syntaxHighlight('
      $this->messages->setBlock(\'messages\');
      $this->messages->getByCode(\'usuario-bloqueado\');');?>
  </p>
  <p>
    Também é possível preencher valores coringas presentes na mensagem do template (%s), para tal, é necessário informar estes parâmetros em um <em>array</em> seguindo o padrão abaixo:
  </p>
  <p>
    <small>Template para exemplo:</small>
    <?=syntaxHighlight('
      {
        "description" : "Template de mensagens para exemplo",
        "alerts" : {
            "codigo" : {
                "style"   : "info",
                "title"   : "%s",
                "message" : "Olá %s %s, como vai?"
            }
        }
      }');?>
    <small>O código resultante seria:</small>
    <?=syntaxHighlight('
      $this->load(\'Modules\Messages\', \'meutemplate\');
      $this->messages->setBlock(\'alerts\');

      $substituir_coringas = array(
        \'title\' => \'Hello World\',
        \'message\' => array(
          \'Bruno\',
          \'Santos\'
        )
      );


      $this->messages->getByCode(\'codigo\', $substituir_coringas);');?>
  </p>