----

## *Storage* {#storage}

### Sessões

Para trabalhar com sessões é necessário utilizar os recursos de *Storage*. Para tal, utiliza-se o objeto *Session* que contém os seguintes métodos:

```php
    class ProdutosController
    {
        public function listarAction()
        {
            $this->load('Storage\Session');

            $this->session->set('key', 'value'); //Cria uma sessão
            echo $this->session->get('key'); //Seleciona uma sessão
            $this->session->clear('key'); //Exclui uma sessão
            print_r($this->session->exists('key')); //Verifica a existência de uma sessão
        }
    }
```

### Cookies

Para trabalhar com *cookies* utiliza-se o objeto *Cookie* que contém os seguintes métodos:

```php
    class ProdutosController
    {
        public function listarAction()
        {
            $this->load('Storage\Cookie');

            $this->cookie->set('key', 'value', '31556926'); //Cria um cookie ($name, $value, $time)
            echo $this->cookie->get('key'); //Seleciona um cookie
            $this->cookie->clear('key'); //Verifica a existência do cookie
            print_r($this->cookie->exists('key')); //Exclui um cookie
        }
    }
```