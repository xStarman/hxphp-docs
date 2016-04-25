----
## Pontapé inicial {#pontape-inicial}
Esta seção aborda o funcionamento do framework e suas características.

----
### Estrutura do framework {#estrutura-do-framework}
O HXPHP tem a seguinte estrutura de pastas:

``` {.brush:php}
  /hxphp/
      app/
        controllers/
        models/
        views/
      public/
        css/
        img/
      src/
        HXPHP/
          System/
            Configs/
              Environments/
              Modules/
            Helpers/
              templates/
            Http/
            Modules/
            Services/
            Storage/
```

A relação das funções de cada pasta pode ser conferida na lista abaixo:
  
+ A pasta `app` é o local de desenvolvimento da sua aplicação;
+ A pasta `public` é o local apropriado para armazenar folhas de estilos, scripts e afins, e;
+ A pasta `src/HXPHP/System/` além de conter os arquivos de controle do framework, também acomoda os Helpers, Módulos e Serviços.
  
----
### Funcionamento da URL {#funcionamento-da-url}

O HXPHP é baseado no padrão de arquitetura de software [MVC](http://pt.wikipedia.org/wiki/MVC) e, por isso, toda a aplicação é controlada pela URL.

Para que você compreenda completamente o funcionamento, tomemos como base dois exemplos:
  
+ a) HXPHP rodando na raiz, e;
+ b) HXPHP rodando em uma subpasta.

Sendo que os endereços de ambos serão, respectivamente:

+ a) `http://site.com.br/`
+ b) `http://localhost/hxphp/`
  
Agora com esses exemplos definidos é importante que você lembre-se da `BASE DA URL`, vista no começo desta documentação. O valor desta configuração de ambiente para os dois exemplos seria, respectivamente:

+ a) `/`
+ b) `/hxphp/`
  
Mas afinal de contas para que serve essa configuração? Pois bem, ela é a fronteira entre o `hostname` e a `requisição`. Ou seja, o que vier depois dela são os parâmetros controladores de nossa aplicação.

Na prática, imagine os seguintes links requisitados por um usuário qualquer em ambos os exemplos:
  
+ http://site.com.br`/`**projetos/listar/1**
+ http://localhost`/hxphp/`**projetos/listar/1**
  
Com as situações ilustradas acima, é possível compreender claramente o processo de separação, visto que, em ambas as situações a requisição foi: `projetos/listar/1`.

Mas, isso não é tudo! Essa requisição é subdividida em:
  
+ `controller` -> *projetos*
+ `action` -> *listar*
+ `parâmetro(s)` -> *1*

#### Subpastas

Também é possível organizar os *controllers* em subpastas. Esta funcionalidade permite uma fácil integração entre back e front-end, por exemplo. Confira o exemplo:
  
+ http://localhost`/hxphp/`**admin/projetos/novo/**
+ http://localhost`/hxphp/`**projetos/listar/1**
  
Essa requisição é subdividida em:

```
  http://localhost/hxphp/admin/projetos/novo/
```
  
+ `subpasta` -> *admin*
+ `controller` -> *projetos*
+ `action` -> *novo*
+ `parâmetro(s)` -> null

**Para utilizar esta funcionalidade necessita-se da existência da respectiva subpasta no diretório dos controllers.**

```
  http://localhost/hxphp/projetos/listar/1
```
  
+ `subpasta` -> null
+ `controller` -> *projetos*
+ `action` -> *listar*
+ `parâmetro(s)` -> *1*
  

O *Controller* e a *Action* tem posição fixa, já os parâmetros são infinitos, ou seja, desde que você especifique os dois primeiros valores da requisição, poderá passar quantos parâmetros desejar!

Exemplo:
`http://site.com.br/controller/action/parametro1/parametro2/parametro3/parametroN`

É válido ressaltar que existe um *Controller* e uma *Action* padrão (IndexController e indexAction, respectivamente), que entra em funcionamento quando a requisição não é específica, por exemplo:

+ http://localhost/hxphp - `Nenhum controller solicitado`, e;
+ http://site.com.br/login/ - `Nenhuma action solicitada`.
    
  
