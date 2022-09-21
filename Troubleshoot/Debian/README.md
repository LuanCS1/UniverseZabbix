<h1 Align="center">TROUBLESHOOT - Somente em Servidores Debian</h1>

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

<pre><code>sudo service zabbix-agent restart</pre></code>

  <li>Verificar a inicialização do serviço.</li><br>

<pre><code>sudo service zabbix-agent status</pre></code>

<b>OBS: sempre reiniciar o serviço duas vezes, pois pode ocorrer algum erro e não “restartar” de primeira.</b></ul>

<h4>2.  Verificar o arquivo de log</h4>

<pre><code>sudo tail /var/log/zabbix/zabbix_agentd.log</pre></code>

<img src="https://olos.attachments.freshservice.com/data/helpdesk/attachments/production/17009248018/original/1625218217519.png?Expires=1663729901&Signature=NGcihW-9qya-vr0oujcKnnBGcj7iT~0UgTUtLXcqm0k8MS82vRNe8vjHbaJV74A54dFIBnVx~j8rM6R47cl04ZYkrOLH5LpevEyNuc1E5oa6Vb0IlgW1SkYzulCaYoS13Vy-iWvza4viY~iyhjRZBUiUJqgG0qt79q2~kDvGrURgqYy4bXlY9FRcH2qZKG3bppD-AcF5n-JQvthGoZ5mhdDsK9qRolBhppgB5aQLon0GFmZdB8LHVe~5R1UI8Hq9Jy2BotqopeaNKL~cqs0S68NgD7PpMqhN2D2pWvhvQFrpAOq-IS7fjjj4H~P4lH91sQJ8wASvqRMDZBgf4jl6CA__&Key-Pair-Id=APKAIPHBXWY2KT5RCMPQ">

<ul>
  <li>Se o servidor Zabbix estiver configurado para auto cadastramento, na primeira tentativa ocorre o erro indicando que o servidor não está ainda cadastrado. Neste caso reiniciar novamente o serviço com o comando abaixo.</li>
<br>
<pre><code>sudo service zabbix-agent restart</pre></code>
</ul>

<h4>3. Nos ultimos casos: </h4>
<ol>
<li> Se ainda esteja com dificuldades é recomendável que verifique se o log do Zabbix está apresentando algum erro.</li><br> 
  <li> Todos os passos até o momento auxiliam na correção de erros de configuração e ativação do serviço do Zabbix no servidor.</li><br> 
  <li> Se ainda estiver com erro que não aponte nada referente a configuração, então pode seguir com a reinstalação do Zabbix como forma mais rápida e garantida de fazer com que funcione corretamente:
  </li></ol><br>

<ul>
<li>Caso queira consultar os pacotes zabbix instalados, isso pode ser feito com o comando abaixo.</li><br>

<pre><code>dpkg -l Zabbix</pre></code>



  <li>O pacote do Agent pode ser desinstalado com o comando abaixo:</li><br>

<pre><code>sudo dpkg -r zabbix-agent</pre></code>


  <li>Executar o comando abaixo para instalar.</li><br>

<pre><code>sudo dpkg -i /home/olos/Downloads/zabbix-agent_3.4.0-1+wheezy_amd64.deb</pre></code>


