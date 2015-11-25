----
## Instalação e Configuração {#instalacao-e-configuracao}

Esta seção contempla os procedimentos de instalação e configuração do framework.

----
### Bootstrapping {#bootstrapping}

Após vistar todos os itens da lista de requisitos, prossiga com a instalação, que tanto em rede local como remota, consiste em:

+ Enviar os arquivos para a pasta desejada;
+ Editar o arquivo `app/config.php`, e;
+ Configuar o .htaccess.

----

#### Configurando o framework

Após enviar os arquivos, configure o framework. Todas as configurações devem ser definidas no arquivo:

`/app/config.php`

As configurações são divididas em dois grupos:

+ Configurações globais, e;
+ Configurações de ambiente.


As `globais` tem como objetivo definir parâmetros que, em suma, não necessitam de alteração, pois, consistem em intervenções no funcionamento padrão do framework.


Já as `configurações de ambiente` são parâmetros que variam de acordo com o `ambiente atual`. O ambiente além de conter a sua configuração padrão também pode conter módulos.


Para iniciar a configuração necessita-se instanciar o objeto `$configs = new HXPHP\System\Configs\Config`. E, após isto, é possível:

+ Alterar as configurações globais, e/ou;
+ Adicionar e configurar ambientes.

Para adicionar um ambiente: `$configs->env->add('ambiente');`. 
Os ambientes padrão são: `development` e `production`.


Para criar um ambiente: 
+ Acesse a pasta `HXPHP\System\Configs\Environments`, e;
+ Crie um arquivo no padrão `EnvironmentNomeDoAmbiente`.

<small>Conteúdo padrão de um ambiente:</small>

``` {.brush:php}
<?php

namespace HXPHP\System\Configs\Environments;

use HXPHP\System\Configs as Configs;

class EnvironmentNomeDoAmbiente extends Configs\AbstractEnvironment
{

}
```


Por padrão, a única configuração padrão de ambiente é a `baseURI` que define a [BASE DA URL](#funcionamento-da-url).
Para alterá-la: `$configs->env->nomedoambiente->baseURI = '/hxphp/';`.


Os módulos padrão de ambiente são: `Database` e `Mail`. Os módulos não são padronizados, portanto a nomenclatura dos métodos é aleatória.

Para criar um módulo é necessário salvá-lo na pasta: `/src/HXPHP/System/Configs/Modules/` e adicioná-lo na lista de módulos do arquivo `/src/HXPHP/System/Configs/RegisterModules.php`.


<small>Exemplo demonstrando o registro de um módulo qualquer chamado Youtube (RegisterModules.php):</small>

``` {.brush:php}
<?php

namespace HXPHP\System\Configs;

class RegisterModules
{
  public $modules = array();

  public function __construct()
  {
    //Informe os nomes dos módulos em lowercase
    $this->modules = array(
      'database',
      'mail',
      'youtube'
    );

    return $this;
  }
}

```
<h5>Exemplo de configuração:</h5>

``` {.brush:php}
<?php
  //Constantes
  $configs = new HXPHP\System\Configs\Config;

  //Globais
  $configs->global->models->directory = APP_PATH . 'models' . DS;

  $configs->global->views->directory = APP_PATH . 'views' . DS;
  $configs->global->views->extension = '.phtml';

  $configs->global->controllers->directory = APP_PATH . 'controllers' . DS;
  $configs->global->controllers->notFound = 'Error404Controller';

  $configs->title = 'Titulo customizado';

  //Configurações de Ambiente - Desenvolvimento
  $configs->env->add('development');

  $configs->env->development->baseURI = '/hxphp/';

  $configs->env->development->database->setConnectionData(array(
    'host' => 'localhost',
    'user' => 'root',
    'password' => '',
    'dbname' => 'hxphp'
  ));

  $configs->env->development->mail->setFrom(array(
    'from' => 'Remetente',
    'from_mail' => 'email@remetente.com.br'
  ));

  //Configurações de Ambiente - Produção
  $configs->env->add('production');

  $configs->env->production->baseURI = '/';

  $configs->env->production->database->setConnectionData(array(
    'host' => 'localhost',
    'user' => 'usuariodobanco',
    'password' => 'senhadobanco',
    'dbname' => 'hxphp'
  ));

  $configs->env->production->mail->setFrom(array(
    'from' => 'Remetente',
    'from_mail' => 'email@remetente.com.br'
  ));


  return $configs;
```

----
<h4>Configurando o .htaccess</h4>

O arquivo `.htaccess` é responsável pelo processo de reescrita das URLs, que é fundamental para a aplicação, ou seja, trata-se do **coração** do mecanismo MVC.


Portanto, edite o arquivo `.htaccess`, localizado na pasta raiz do framework, para configurar a BASE. O bloco que deve ser configurado é listado abaixo:


``` {.brush:php}
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /hxphp/
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /hxphp/index.php [L]
  </IfModule>
```


Atente-se para os comandos: `RewriteBase` e `RewriteRule`, nota-se que ambos contém a menção do valor da `BASE_URL`, portanto, caso você tenha definido um valor customizado, altere-o. Adotando nosso exemplo anterior, tendo a `BASE_URL` igual a *administrativo*, tem-se o seguinte código:


``` {.brush:php}
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /administrativo/
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /administrativo/index.php [L]
  </IfModule>
```


Para testar o sucesso da configuração, recomenda-se que acesse: 
`http://localhost/{BASE_URL}/`

----

### Instalando as dependências do framework {#instalando-as-dependencias}

O HXPHP Framework utiliza o [Composer](https://getcomposer.org/download) para gerenciar as dependências, portanto, antes de prosseguir, certifique-se que o mesmo está instalado em sua máquina. Para isso, abra o terminal (CMD no Windows) e digite o seguinte comando: `composer --version`.


Após constatar que o [Composer](https://getcomposer.org/download) está em pleno funcionamento, navegue até a pasta do framework e execute o comando: `composer install`.
