----

<h2 id="storage">*Storage*</h2>

Para trabalhar com sessões no HXPHP Framework é necessário utilizar os recursos de *Storage*. Para tal, utiliza-se o objeto *Session* que contém os seguintes métodos:
+ `set($name, $value)` - Cria uma sessão;
+ `get($name)` - Seleciona uma sessão;
+ `exists($name)` - Verifica a existência de uma sessão, e;
+ `clear($name)` - Exclui uma sessão.