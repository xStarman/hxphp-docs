<hr class="col-md-12">
<h2 id="tools"><em>Tools</em></h2>
<p>
Nesta seção você ir&aacute; conhecer o objeto auxiliar <em>Tools</em>.
</p>
<hr class="col-md-12">
<h3 id="tools">Métodos auxiliares</h3>
<p>
O objeto <em>Tools</em> é uma ferramenta de apoio geral, ou seja, trata-se de um objeto que contém métodos comuns com funções de filtro, tratamento, criptografia e afins.
</p>
<p>
Por padrão, este objeto contém os seguintes métodos <strong>estáticos</strong>:
<ul>
<li><code>dd($data, $dump = false)</code> - Exibe os dados;</li>
<li><code>hashHX($password, $salt = null)</code> - Criptografa a senha do usuário no padrão HXPHP, e;</li>
<li><code>filteredName($input)</code> - Processo de tratamento para o mecanismo MVC.</li>
</ul>
</p>
<p>
  <small>Tools na prática:</small>
  <?=syntaxHighlight('\HXPHP\System\Tools::hashHX($post[\'password\']); ');?>
</p>