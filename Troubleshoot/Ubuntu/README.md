<h1 Align="center">TROUBLESHOOT - Somente em Servidores Ubuntu</h1>


<h3>Ter uma base do Ubuntu será necessária de agora adiante nos servidores Zabbix.</h3>



<h4>Bom, primeiramente é interessante falarmos sobre comandos básicos utilizados e o que fazem:</h4>


<ul>
  <li><b>Sudo</b> - O comando sudo (SuperUser DO) permite que você execute programas ou outros comandos com privilégios administrativos, 
    assim como "Executar como administrador" no Windows. Ele é útil quando, por exemplo, você precisa modificar arquivos em um diretório, 
    ao qual seu usuário normalmente não teria acesso.</li><br>



  <li><b>Apt-get</b> - Um dos comandos mais importantes do Ubuntu. Ele é usado para instalar, atualizar e remover qualquer pacote. O apt-get funciona basicamente em um banco de dados de pacotes disponíveis.
Aqui está a lista de diferentes comandos do apt-get:</li><br>

<pre><code>sudo apt-get update</pre></code>

<i>O update atualiza o banco de dados e informa ao seu sistema se há pacotes mais novos disponíveis ou não.</i>

<pre><code>sudo apt-get upgrade</pre></code>

<i>Com este comando, serve como um próximo passo depois do update e acaba a atualizar os pacotes.</i>

<pre><code>sudo apt-get upgrade nome do pacote</pre></code>

<i>Pode utilizar esse comando mudando o "nome do pacote" para o nome do pacote desejado, assim atualizará individualmente.</i>

<pre><code>sudo apt-get install nome do pacote</pre></code>

<i>Pode utilizar esse comando mudando o "nome do pacote" para o nome do pacote desejado, assim irá instalar o pacote.</i>



  <li>ls - Lista todos os arquivos.</li><br>


  <li>cd - Navega pelos diretórios do Linux em geral. </li><br>

  <p><b>cd / </b>- Leva você ao diretório raiz. <br>
    <b>cd .. </b>- Leva você até um nível de diretório. <br>
    <b>cd -</b> - Leva você ao diretório anterior.</p>


<li>pwd - Exibe o caminho completo do diretório de trabalho atual</li><br>



<li>cp - Serve para copiar um arquivo, porém deve especificar tanto o arquivo que deseja copiar, quanto o local onde deseja copia-lo. Exemplo:</li><br>

<pre><code>cp xyz / home / myfiles copia o arquivo " xyz " para o diretório "/ home / myfiles "</pre></code>


  <li>mv - Move os arquivos, é necessário especificar da mesma maneira que o copiar.</li><br>



  <li>rm - remove o arquivo especificado.</li><br>

  <p><b>rmdir ("remove directory") </b>- Remove um diretório vazio.<br>
    <b>rm -r ("remove recursively")</b> - Remove um diretório junto com seu conteúdo.</p>


  <li>mkdir - Permite criar um novo diretório.</li>

  <li>history - exibe todos os seus comandos anteriores, até o limite do histórico.</li>

  <li>htop - exibe os processos usando os recursos do sistema.</li>



  <li>chmod - É usado para ler, escrever e executar permissões de arquivos e diretórios. Exemplo:</li>

<pre><code>sudo chmod 777 Nome do Arquivo</pre></code>





  <li>Para ver a versão do Linux:</li><br>

<pre><code>cat /etc/*-release</pre></code>



<h3>No caso do Zabbix não estar funcionando corretamente, siga os passos abaixo:</h3>

<h4>1. Revisar a configuração do Zabbix Agent</h4>
<ul>
  <li>
    Editar o arquivo <b>/etc/zabbix/zabbix_agentd.conf</b> com algum editor de texto (ex. nano, vi, vim).</li><br>

<pre><code>sudo nano /etc/zabbix/zabbix_agentd.conf</pre></code>

  <li> Editar os parâmetros abaixo.</li>

  <p><li>Alterar parâmetro Server</li>
    <b>Server=Incluir o Ip do Proxy do ambiente que o cliente está.</b><br></p>

  <li>Alterar parâmetro ServerActive</li>

  <b>ServerActive= Incluir o Ip do Proxy do ambiente que o cliente está.</b>

  <li>Comentar parâmetro Hostname inserindo o caractere “#” na frente</li>

  <b>#Hostname=Zabbix Server</b><br>

  <li>Alterar o parâmetro HostMetadataItem</li>

  <b>HostMetadataItem=system.uname</b><br>

  <li> Reiniciar o serviço do Zabbix Agent.</li><br>

<pre><code>sudo systemctl restart zabbix-agent</pre></code>

  <li>Verificar a inicialização do serviço.</li><br>

<pre><code>sudo systemctl status zabbix-agent</pre></code>

<b>OBS: sempre reiniciar o serviço duas vezes, pois pode ocorrer algum erro e não “restartar” de primeira.</b></ul>

<h4>2.  Verificar o arquivo de log</h4>

<pre><code>sudo tail /var/log/zabbix/zabbix_agentd.log</pre></code>


Exemplo de inicialização correta. A mensagem de que o arquivo com o PID não está disponível pode ser ignorada se houver a indicação de que o serviço está active (running).
<img src="https://olos.attachments.freshservice.com/data/helpdesk/attachments/production/17009248018/original/1625218217519.png?Expires=1663729901&Signature=NGcihW-9qya-vr0oujcKnnBGcj7iT~0UgTUtLXcqm0k8MS82vRNe8vjHbaJV74A54dFIBnVx~j8rM6R47cl04ZYkrOLH5LpevEyNuc1E5oa6Vb0IlgW1SkYzulCaYoS13Vy-iWvza4viY~iyhjRZBUiUJqgG0qt79q2~kDvGrURgqYy4bXlY9FRcH2qZKG3bppD-AcF5n-JQvthGoZ5mhdDsK9qRolBhppgB5aQLon0GFmZdB8LHVe~5R1UI8Hq9Jy2BotqopeaNKL~cqs0S68NgD7PpMqhN2D2pWvhvQFrpAOq-IS7fjjj4H~P4lH91sQJ8wASvqRMDZBgf4jl6CA__&Key-Pair-Id=APKAIPHBXWY2KT5RCMPQ">

 <ul>
  <li>Se o servidor Zabbix estiver configurado para auto cadastramento, na primeira tentativa ocorre o erro indicando que o servidor não está ainda cadastrado. Neste caso reiniciar novamente o serviço com o comando abaixo.</li>
<br>
<pre><code>sudo systemctl restart zabbix-agent</pre></code>
</ul>



<h4>3. Nos ultimos casos: </h4>
<ol>
<li> Se ainda esteja com dificuldades é recomendável que verifique se o log do Zabbix está apresentando algum erro.</li><br> 
  <li> Todos os passos até o momento auxiliam na correção de erros de configuração e ativação do serviço do Zabbix no servidor.</li><br> 
  <li> Se ainda estiver com erro que não aponte nada referente a configuração, então pode seguir com a reinstalação do Zabbix como forma mais rápida e garantida de fazer com que funcione corretamente:
  </li></ol><br>

<ul>
<li>Caso queira consultar os pacotes zabbix instalados, isso pode ser feito com o comando abaixo.</li><br>

<pre><code>sudo dpkg -l | grep Zabbix</pre></code>



  <li>O pacote do Agent pode ser desinstalado com o comando abaixo:</li><br>

<pre><code>sudo apt-get purge zabbix-agent</pre></code>


  <li>Executar o comando abaixo para instalar, ubuntu em geral e ubuntu focal:</li><br>
<pre><code>sudo dpkg -i zabbix-agent_5.0.23-1+focal_amd64.deb</pre></code>

<pre><code>sudo dpkg -i zabbix-agent_5.4.12-1+ubuntu20.04_arm64.deb</pre></code>
