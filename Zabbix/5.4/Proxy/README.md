<!DOCTYPE html>
<h1 align="center">Zabbix Proxy: Como funciona?</h1>


<blockquote align="center">
"Está parte é apenas para entendimento, existe um artigo somente para explicar o Zabbix Proxy."</blockquote>
Proxy é o termo utilizado para definir os intermediários entre o usuário e seu servidor, sendo assim, utilizamos o Proxy dos ambientes para fazer as comunicações entre servidores e o Zabbix.<br><br>

<img align="center" src="https://user-images.githubusercontent.com/75817948/192020599-1f5a7bb8-fda2-4aed-b198-cee3bdfee711.png">
<h5 align="center">Apenas para exemplificar como funciona o Zabbix Proxy</h5>
 

Como funciona o Zabbix Proxy, como na imagem acima, ele fica no ambiente do cliente e colhe as informações dos Zabbix Agents, e através de portas permitidas pelas configurações do Firewall, ele entrega essas informações colhidas ao Zabbix Server.

<h1 align="center">Instalação do Zabbix Proxy do Zero</h1>

<h4>Primeiramente, só iremos instalar o Zabbix Proxy, do zero, caso seja a ultima opção.</h4>

<b>Observação: As senhas estão com 'senha123' para segurança do artigo.</b>

<ol>
  <li>  Iremos fazer o Linux fazer o download da aplicação do proxy com o seguinte comando:</li>
<pre><code>wget https://repo.zabbix.com/zabbix/5.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.4-1+ubuntu$(lsb_release -rs)_all.deb</pre></code>
<li>
  Após o download, iremos fazer a instalação do pacote que fizemos download:</li>
<pre><code>sudo dpkg -i zabbix-release_5.4-1+ubuntu$(lsb_release -rs)_all.deb</pre></code>



<li>Feito isto, iremos atualizar os pacotes, e em seguida instalar o Banco de dados do Proxy e o seu Script.</li>
<pre><code>sudo apt update
sudo apt -y install zabbix-proxy-mysql
sudo apt -y install zabbix-sql-scripts</pre></code>


Observação: Nós utilizamos na versão atual o mariadb e para ele é necessário o script, mas a versão SQLite já vem com o script.


<li>Após este processo, iremos instalar o Banco de dados definitivamente(MariaDB), comandos para a instalação:</li>

<pre><code>sudo apt -y install mariadb-common
sudo apt -y install mariadb-client-10.3
sudo apt -y install mariadb-server-10.3

sudo apt -y install mariadb-client
sudo apt -y install mariadb-server</pre></code>



<li>Depois da Instalação do MariaDB, iremos fazer a configuração do Banco, utilizando o comando abaixo iremos configurar a segurança do banco:</li>

<pre><code>sudo mysql_secure_installation</pre></code>



<li>Após o comando acima, iremos seguir o passo a passo abaixo, em negrito a ação:</li>
  <pre><code>Enter current password for root: <b>Press Enter</b>
Set root password? [Y/n]: <b>Y</b>
New password: <b>Enter root DB password</b>
Re-enter new password: <b>Repeat root DB password</b>
Remove anonymous users? [Y/n]:<b> Y</b>
Disallow root login remotely? [Y/n]: <b>Y</b>
Remove test database and access to it? [Y/n]: <b> Y</b>
Reload privilege tables now? [Y/n]: <b> Y</b></pre></code>

<li>Feito o processo acima, iremos logar no banco de dados:</li>

<pre><code>sudo mysql -uroot -p'senha123'</pre></code>



<li>Dentro do mySQL, iremos fazer os seguintes comandos:</li>

<pre><code>create database zabbix_proxy character set utf8 collate utf8_bin;

grant all privileges on zabbix_proxy.* to zabbix@localhost identified by 'zabbixDBpass';

quit;</pre></code>


<li>Após este processo, iremos importar o Esquema de banco de dados, que seria a estrutura do banco.</li>

<pre><code>zcat /usr/share/doc/zabbix-sql-scripts/mysql/schema.sql.gz | mysql -uzabbix -p'zabbixDBpass' zabbix_proxy

sudo mysql -uroot -p</pre></code>


<li>Voltamos a configuração do Zabbix Proxy, antes de irmos para o arquivo de configurações, devemos criar o arquivo PSK que é o Certificado utilizado no Zabbix Server.:</li>

<pre><code>sudo nano /etc/zabbix/psk.key</pre></code>

Observação: Colocar a chave do certificado.



<li>Feito isto, vamos seguir adiante, e agora iremos configurar o arquivo do Zabbix proxy, para acessarmos este arquivo, utilizamos o comando abaixo:</li>

<pre><code>sudo nano /etc/zabbix/zabbix_proxy.conf</pre></code>


<li>No arquivo de configurações iremos mudar algumas opções:</li>
<pre><code>DBPassword=zabbixDBpass
ConfigFrequency=120
Server= 127.0.0.1
Hostname=<Preencher com o hostname padrão Olos>
TLSConnect=psk 
TLSAccept=psk
TLSPSKIdentity=zabbix.com.br
TLSPSKFile=/etc/zabbix/psk.key
Observação: As opções PSK são referentes ao certificado.</pre></code>
<li>Seguindo no arquivo de configurações do proxy, iremos alterar algumas opções que irão otimizar o desempenho do proxy:</li>
<pre><code>StartPollers=100
StartPollersUnreachable=50
StartPingers=50
StartTrappers=10
StartDiscoverers=15
StartPreprocessors=15
StartHTTPPollers=5</pre></code>

Após estás configurações você irá salvar utilizando o <b>CTRL + O</b> e depois <b>CTRL + X</b> para fechar o documento:


<li>Após este processo iremos fazer um restart do serviço do proxy e habilita-lo para iniciar automaticamente sempre que o servidor for desligado, assim a monitoria não irá parar:</li>
<pre><code>sudo systemctl restart zabbix-proxy
sudo systemctl enable zabbix-proxy</pre></code>




<li>Com isto, finalizamos a configuração do proxy, no entanto a configuração não terminou, iremos agora instalar o ODBC para o monitoramento de Query e Bases do Banco de Dados do servidores:</li>
<pre><code>sudo apt install unixodbc
sudo apt install unixodbc-dev</pre></code>


<li>Iremos fazer o download do plugin do mysql, que é o MySQLConnector:</li>

<pre><code>sudo wget https://repo.mysql.com/apt/ubuntu/pool/mysql-8.0/m/mysql-community/mysql-community-client-plugins_8.0.27-1ubuntu20.04_amd64.deb<br>

sudo dpkg -i mysql-community-client-plugins_8.0.27-1ubuntu20.04_amd64.deb

sudo wget https://dev.mysql.com/get/Downloads/Connector-ODBC/8.0/mysql-connector-odbc_8.0.27-1ubuntu20.04_amd64.deb

sudo dpkg -i mysql-connector-odbc_8.0.27-1ubuntu20.04_amd64.deb

sudo apt --fix-broken install

sudo wget https://dev.mysql.com/get/Downloads/Connector-ODBC/8.0/mysql-connector-odbc_8.0.27-1ubuntu20.04_amd64.deb

sudo dpkg -i mysql-connector-odbc_8.0.27-1ubuntu20.04_amd64.deb</pre></code>




<li>Iremos fazer a instalação do <b>cURL</b> que serve para transferir dados  utilizando URL, usaremos ele para pacotes:</li>

Comando para instalação do <b>cURL</b>:
<pre><code>sudo apt install curl</pre></code>

Comando para a instação dos pacotes:
<pre><code>curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit</pre></code>

<li>Após este processo, iremos atualizar os pacotes e instalar o pacote ODBC17:</li>
<pre><code>sudo apt update

sudo ACCEPT_EULA=Y apt-get install -y msodbcsql17</pre></code>



<li>Configurado o ODBC, primeiramente vamos validar o connector usando o comando abaixo:</li>
<pre><code>
sudo nano /etc/odbcinst.ini</pre></code>

Observação: Irá abrir uma tela com estes dados:


<pre><code>[MySQL_Unicode]
Driver=/usr/lib/x86_64-linux-gnu/odbc/libmyodbc8w.so
UsageCount=1

[MySQL_ANSI]
Driver=/usr/lib/x86_64-linux-gnu/odbc/libmyodbc8a.so
UsageCount=1

[MSSQLSERVER17] - Tenha extrema atenção com isto, o nome deve bater com o que está no odbc.ini
Description=Microsoft ODBC Driver 17 for SQL Server
Driver=/opt/microsoft/msodbcsql17/lib64/libmsodbcsql-17.9.so.1.1
UsageCount=1</pre></code>




<li>E depois disto iremos configurar o banco de dados que queremos monitorar utilizando o comando abaixo:</li>

<pre><code>sudo nano /etc/odbc.ini</pre></code>

Observação: Irá abrir uma tela com estes dados:


<pre><code>[MSSQLSERVER]
Driver = MSSQLSERVER17 - <b>Tenha extrema atenção com isto, o nome deve bater com o que está no odbcinst.ini</b>
Description = ODBC SQL Server
Trace = yes
TraceFile = /tmp/odbc_sqlserver.log
Server = 10.30.11.41\\MSSQLSERVER,1433
User = Zabbix
Password = *******************
</pre></code>



<li>Feito este processo, iremos configurar as permissões SSL:</li>

<pre><code>sudo nano /etc/ssl/openssl.cnf</pre></code>


<li>Após abrir o arquivo acima, iremos adicionar linhas no arquivo:</li>



- Adicionaremos a seguinte linha no começo/topo do arquivo: <b>openssl_conf = openssl_init</b>
<pre><code>openssl_conf = openssl_init</pre></code>



-  E Adicionaremos as linhas seguintes no final/base do arquivo:
<pre><code>[openssl_init]
ssl_conf = ssl_sect

[ssl_sect]
system_default = system_default_sect

[system_default_sect]
CipherString = DEFAULT@SECLEVEL=1
</pre></code>



<li>Para testarmos o ODBC, precisamos alterar o nome MSSQLSERVER para o nome do Servidor. Comando abaixo:</li>
<pre><code>isql -v MSSQLSERVER Zabbix senha123</pre></code>

<b>Observação: Não entrarei mais afundo no assunto ODBC, pois existe um tópico somente para isto.
