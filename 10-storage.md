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

            $this->session->set('key', 'value');
            echo $this->session->get('key');
            $this->session->clear('key');
            print_r($this->session->exists('key'));
        }
    }
```
+ `set($name, $value)` - Cria uma sessão;
+ `get($name)` - Seleciona uma sessão;
+ `exists($name)` - Verifica a existência de uma sessão, e;
+ `clear($name)` - Exclui uma sessão.