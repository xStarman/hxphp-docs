----
<h2 id="tools">*Tools*</h2>

Nesta seção você ir&aacute; conhecer o objeto auxiliar *Tools*.
----
<h3 id="tools">Métodos auxiliares</h3>

O objeto *Tools* é uma ferramenta de apoio geral, ou seja, trata-se de um objeto que contém métodos comuns com funções de filtro, tratamento, criptografia e afins.

Por padrão, este objeto contém os seguintes métodos <strong>estáticos</strong>:

+ `dd($data, $dump = false)` - Exibe os dados;
+ `hashHX($password, $salt = null)` - Criptografa a senha do usuário no padrão HXPHP, e;
+ `filteredName($input)` - Processo de tratamento para o mecanismo MVC.


Tools na prática:
<?=syntaxHighlight('\HXPHP\System\Tools::hashHX($post[\'password\']); ');?>