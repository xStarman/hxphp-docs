----

## *Storage* {#storage}

### Sessões

Para trabalhar com sessões no HXPHP Framework é necessário utilizar os recursos de *Storage*. Para tal, utiliza-se o objeto *Session* que contém os seguintes métodos:

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
