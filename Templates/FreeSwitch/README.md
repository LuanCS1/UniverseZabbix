<!DOCTYPE html>
<html lang="PT">
  <body>
<h1 align="center" font-family="verdana" ><b>Freeswitch</b></h1><br>

<h3 align="left" color="pink">Função do Template</h3>
<b><p>Monitores:</b> uptime do serviço, taxa de sessão, chamadas ativas, sessões ativas, sessões de pico, chamadas de pico e número de processos Freeswitch. Acionar em muitas chamadas ativas [ou seja, quando você está ficando sem canais].

Requer que o subagente <b>SNMP mod_snmp</b> esteja habilitado no <b>Freeswitch+net-snmp</b>.<br><br><br>

<h2 align="center" >===============Procedimento Ativação SNMP FS===============</b></h2>

  <p>Compilação e instalação do <b><span>mod_snmp</span></b><br>
    Habilite a compilação do módulo por padrão, editando o arquivo <b><span>modules.conf</span></b>.</p>
<b><h4 align="center">Compile mod_snmp</b></h4>

<pre><code>cd /usr/src/freeswitch
nano modules.conf 
or 
sudo nano /usr/src/freeswitch/modules.conf</pre></code>
<ul>
  <li>Descomente a linha abaixo em <b>modules.conf</b>.:</li><br>

<blockquote>event_handlers/mod_snmp</blockquote>

<li>Compile e instale.</li><br>

<pre><code>sudo make mod_snmp-install</pre></code>

<li>Para habilitar o carregamento automático do módulo, 
  edite o arquivo <b>/usr/local/freeswitch/conf/autoload_configs/modules.conf.xml</b> </li>

<pre><code>sudo nano /usr/local/freeswitch/conf/autoload_configs/modules.conf.xml</pre></code>

<li>E adicione a linha abaixo.</li><br>

<pre><code><.load module="mod_snmp"></pre></code>

<b><h2 align="center"> Instalação e configuração do serviço snmpd</b></h2>
 <li>Crie o arquivo <b>/etc/apt/sources.list.d/snmpd.list</b> e insira as linhas abaixo com as URLs do repositório.</li><br>

<pre><code>sudo nano /etc/apt/sources.list</pre></code>

<h3 align="left">non-free</h3>
<pre><code>deb http://ftp.us.debian.org/debian/ bullseye non-free
deb-src http://ftp.us.debian.org/debian/ bullseye non-free</pre></code>

<p>O exemplo acima refere-se à instalação no <b>Debian 11 (bullseye)</b>.<br>
Para outras versões de Debian, é necessário trocar <b>'bullseye'</b> para o codinome correspondente.</p>

<h3 align="left">Instale os pacotes necessários.</h3>

<pre><code>sudo apt-get update
sudo apt-get install snmp snmp-mibs-downloader snmpd</pre></code>

<li>Edite o arquivo de configuração <b>/etc/snmp/snmpd.conf</b></li><br>

<pre><code>sudo nano /etc/snmp/snmpd.conf</pre></code>

<li>Adicionando ou efetuando a troca das variáveis abaixo e fazendo ajustes se necessário (ex. agentXPerms).
Add this:</li><br>

<pre><code># --- #
Run as an AgentX master agent</h3>
master agentx

# Listen on default named socket /var/agentx/master
# agentXPerms SOCKPERMS [DIRPERMS [USER|UID [GROUP|GID]]]
agentXPerms 0755 0755 freeswitch daemon
agentXPerms 0755 0755 freeswitch freeswitch

agentaddress 0.0.0.0,[::]

rocommunity public
# --- #
</pre></code>

<p>Na variável agentXPerms devem ser especificados o username e group sob os quais o serviço freeswitch é executado.<br>
Por exemplo, em servidores, em geral o usuário/grupo é de acordo com o configurado por você.</p>

  <li><h4 align="left">Reinicie o serviço de snmpd.</h4></li>

<pre><code>sudo systemctl restart snmpd
and
sudo /etc/init.d/snmpd restart</pre></code>

  <li>Altere permissões do diretório abaixo.</li><br>

<pre><code>sudo chmod 755 /var/agentx/</pre></code>

  <h3 align="left">Configuração de firewall</h3>
<p>Para permitir a monitoração remota por ferramentas como o Zabbix, a porta 161/UDP deve estar autorizado no firewall.<br>
O exemplo abaixo apresenta os comandos para fazer a habilitação e a verificação das regras ativas em Debian 11 com firewalld.</p>

<pre><code>sudo firewall-cmd --add-service snmp --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-all</pre></code>

  <li>Reinicie o Freeswitch.</li><br>

<pre><code>sudo systemctl restart freeswitch</pre></code>

<h3 align="left">Verificação do funcionamento</h3>
<blockquote>
# Check if the agentX is connected to the FS in the FS log (fs_cli):

[INFO] mod_snmp.c:70 NET-SNMP version 5.9 AgentX subagent connected
</blockquote>
  <b><h4>Para testar:</h4></b>

<pre><code>snmpwalk -v 1 -c public localhost .1.3.6.1.4.1.27880
snmpwalk -v 1 -c public localhost .1.3.6.1.4.1.27880.1.2</pre></code>
  </body>
<h3 align="left"> More details:</h3>
  
<blockquote>
https://freeswitch.org/confluence/display/FREESWITCH/mod_snmp<br>
https://freeswitch.org/confluence/display/FREESWITCH/SNMP<br>
https://wiki.kolmisoft.com/index.php/Freeswitch_SNMP
  </blockquote>
<h2 align="center">Configuração Zabbix</h2>
<p>No Zabbix precisará colocar a comunicação via SNMP:<p>
<ol>
  
  <li><p>Atrelando a comunicação via SNMP com a <b>porta 161</b>:</p>
<img src="https://raw.githubusercontent.com/LuanCS1/ProcessamentoDImagens/main/ConfigFreeSwitch.png"></li>
  <li><p>Finalizando o atrelamento com a versão do SNMP e a comunicação:</p>
<img src="https://raw.githubusercontent.com/LuanCS1/ProcessamentoDImagens/main/ConfigFreeSwitch2.png"></li>

