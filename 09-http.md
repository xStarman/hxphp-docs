    <hr class="col-md-12">
    <h2 id="http"><em>HTTP</em></h2>
    <p>
        Nesta seção você ir&aacute; conhecer o processo de obtenção de dados via requisições HTTP.
    </p>
    <hr class="col-md-12">
    <h3 id="request">HTTP Request</h3>
    <p>
        Para a obtenção de dados de requisições como <strong>POST</strong> e <strong>GET</strong> utiliza-se o objeto injetado <em>Request</em>.
    </p>
    <p>
        As vantagens são muitas, dentre as principais pode-se listar a maior segurança, visto que os dados são tratados nativamente.
    </p>
    <p>
        O objeto <em>Request</em> contém os seguintes métodos:
        <ul>
            <li><code>setCustomFilters()</code>;</li>
            <li><code>get()</code>;</li>
            <li><code>post()</code>;</li>
            <li><code>getMethod()</code>;</li>
            <li><code>isPost()</code>;</li>
            <li><code>isGet()</code>;</li>
            <li><code>isPut()</code>, e;</li>
            <li><code>isHead()</code>.</li>
        </ul>
    </p>
    <p>
        O método <code>setCustomFilters()</code> tem o objetivo de customizar os filtros de tratamento aplicados aos dados das requisições. Para tal, é necessário informar um <strong><em>array</em> associativo</strong> com o índice igual ao nome do campo e o valor com a constante de filtro. Leia: <a href="http://php.net/manual/en/filter.filters.sanitize.php" target="_blank">http://php.net/manual/en/filter.filters.sanitize.php</a>. Por padrão, o filtro aplicado em todos os dados é o <code>FILTER_SANITIZE_STRING</code> que evita a injeção de códigos HTML.
    </p>
    <p>
      <small>O código resultante seria:</small>
      <?=syntaxHighlight('
          <?php
            class ProdutosController extends \HXPHP\System\Controller
            {
                
                public function salvarAction()
                {
                    $this->request->setCustomFilters(array(
                        \'valor\' => FILTER_SANITIZE_NUMBER_FLOAT
                    ));

                }

            }

       ');?>
    </p>
    <p>
        Já os métodos <code>get()</code> e <code>post()</code> retornam os dados filtrados, sendo que é possível retornar todo o conteúdo ou apenas um dado específico.
    </p>
    <p>
      <small>O código resultante seria:</small>
      <?=syntaxHighlight('
          <?php
            class ProdutosController extends \HXPHP\System\Controller
            {

                public function salvarAction()
                {
                    $this->request->setCustomFilters(array(
                        \'valor\' => FILTER_SANITIZE_NUMBER_FLOAT
                    ));

                    $this->request->post(); //Array com todos os dados enviados
                    $this->request->post(\'valor\'); //Apenas o conte&uacute;do do campo valor

                }

            }

       ');?>
    </p>
    <p>
        E, por fim, tem-se os métodos: <code>getMethod()</code>, <code>isPost()</code>, <code>isGet()</code>, <code>isPut()</code> e <code>isHead()</code> que tem como função determinar qual é o método de requisição.
    </p>