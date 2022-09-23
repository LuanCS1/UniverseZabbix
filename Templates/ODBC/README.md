<h1 Align="center"> Zabbix Agent em ODBC</h1>

<p>Para colocar o Zabbix para monitorar o ODBC(iremos incluir em todos os DTB que estiver no cliente), será necessário criar um usuário Zabbix, que irá logar como as permissões adequadas e irá enviar a informação para o Zabbix Proxy.

Assim que logar no ODBC, irá criar uma New Query e colocar o script abaixo:</p>
<ul>
  <li>
    Cria login SQL: </li>

<pre><code>USE [master]; IF (SUSER_ID('Zabbix') IS NOT NULL) DROP LOGIN [Zabbix];
CREATE LOGIN [Zabbix] WITH PASSWORD = 0X02004D87566F35F8EBED34A0BF27BCCBE12D40481D3DA70CFEC58546B75BB09024EDDAB949AB145D6470F73C2D8B2C663672C509A0613846FD41A42DBE8B50D2E30BF0C45245 HASHED, DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE = [us_english], CHECK_POLICY = OFF, CHECK_EXPIRATION = OFF;
</pre></code>

<li>Após a criação do Login SQL, iremos dar permissões para este usuário que criamos, com o script abaixo:

  Permissões na instancia: </li>


<pre><code>USE [master]; GRANT CONNECT SQL TO [Zabbix]; USE [master]; GRANT VIEW SERVER STATE TO [Zabbix];
DECLARE @create_user VARCHAR(MAX) = ''
SELECT @create_user = @create_user + 'USE [' + name + ']; IF EXISTS(SELECT * FROM sys.database_principals WHERE name = ''Zabbix'') DROP USER [Zabbix]; CREATE USER [Zabbix] FOR LOGIN [Zabbix]; ' FROM sys.databases
EXEC(@create_user)
USE [msdb]; ALTER ROLE [db_datareader] ADD MEMBER [Zabbix] USE [master];ALTER ROLE [db_datareader] ADD MEMBER [Zabbix]</pre></code>


Após dar a permissão, finalizamos o procedimento no MYSQL.

Agora será necessário verificar o Proxy, será necessário logar no proxy do ambiente em que o DTB está.

 Após logar no Proxy, será necessário incluir aquele ODBC para monitoria, e para isto utilizamos o seguinte comando:



<pre><code>vim /etc/odbc.ini</pre></code>


Após colocar este comando, a tela ficará assim:<br>
<img src="https://user-images.githubusercontent.com/75817948/192009811-2ae47ca9-0dd4-4ec7-a705-d2bf8fab29b2.png">


Com isso, você irá incluir a informações, só copiar e colar a descrição abaixo, e para inserir no Linux aperte a tecla I, que da o INSERT:

<pre><code>
[MSSQLSERVER] 
Driver = MSSQLSERVER17
Description = ODBC SQL
Server Trace = yes
TraceFile = /tmp/odbc_sqlserver.log
Server = IPDOBANCODEDADOS\\MSSQLSERVER,1433
User = Zabbix
Password = senha@123
</pre></code>
Feito isso, você irá precisar alterar algumas coisas na descrição e colocar do DTB que está sendo configurado.   


<pre><code>
[MSSQLSERVER] | <b><i> Coloca o Hostname do Servidor que está incluindo no lugar do MSSQLSERVER, exemplo: LUANCSDTB</b></i>
Driver = MSSQLSERVER17 |<b><i> Não muda</b></i>
Description = ODBC SQL |<b><i> Não muda</b></i>
Server Trace = yes |<b><i> Não muda</b></i>
TraceFile = /tmp/odbc_sqlserver.log |<b><i> Não muda</b></i>
Server = IPDOBANCODEDADOS\\MSSQLSERVER,1433 |<b><i> Coloca o IP do host\\Hostname do Servidor, mantém a porta 1433</b></i>
User = Zabbix | <b><i>Não muda</b></i>
Password = senha@123 |<b><i> Não muda</b></i>
</pre></code>

Após isto, ele ficará assim:<br>
<img src="https://user-images.githubusercontent.com/75817948/192010893-06133182-d976-4720-abcf-47564ad8051a.png">


Há casos em que o cliente tem mais de um DTB, e nesses casos, fica mais prático, após ter configurado um deles, duplicar as informações, como exemplo abaixo, única coisa que irá alterar é o Hostname, Server = IP\\Hostname.

  Após isto, aperte <b>ESC</b>, dois pontos <b>":"</b> , <b>wq</b>.



Assim, foi salvo a alteração. Porém não terminou, antes encerrar o acesso ao proxy, vamos testar a comunicação com o ODBC e para isto, utilizamos o comando abaixo: 



<pre><code>isql -v MSSQLSERVER Zabbix senha@123 </pre></code>


Para o comando funcionar é necessário alterar o MSSQLSERVER e colocar o Hostname do servidor DTB que quer testar a comunicação.



Exemplo: isql -v LUANCSDTB Zabbix senha@123



Após isto, ele apresentará está tela: <br>
<img src="https://user-images.githubusercontent.com/75817948/192011213-8395442e-f280-4b6f-a947-060f13113369.png">


Faremos outro teste para saber se realmente o ODBC está funcionando, nesse teste utilizamos o comando abaixo:

<pre><code>Select @@VERSION</pre></code>
Irá aparece uma tela como está abaixo:<br>

<img src="https://user-images.githubusercontent.com/75817948/192011366-bc40947d-4ed1-47b9-a91f-51f206409e87.png">


Com esse teste, podemos confirmar que alem da permissão de conexão, esse user consegue fazer o select que queremos.

Feito isso, pode encerrar a conexão, para sair do SQL, escreva quit. 

E para sair do Linux, escreva exit. 



Vamos incluir o ODBC no nosso Zabbix.



No Zabbix, iremos ir em Configuration, Host, e procuraremos o servidor que está querendo colocar o ODBC para monitorar.

Iremos entrar no Host e iremos em MACRO. assim que entrar na tela Macro no espaço Macro, iremos colocar esse comando:
 

<pre><code>{$DSN}</pre></code>


E em Value, iremos colocar o Hostname do Servidor. como imagem abaixo:<br>
<img src="https://user-images.githubusercontent.com/75817948/192011629-3ba79f28-85fa-46cc-8d40-b06c5b9c2006.png">


Feito isto, só clicar em update.



Opa, antes disso, vai no Latest Data e ve se começa a brotar informações do ODBC desse servidor (demora um pouco), se chegar as informações como exemplo abaixo, está finalizado!<br>
<img src="https://user-images.githubusercontent.com/75817948/192011847-9f33ce24-a4a1-492c-8c7b-6f5c5071321d.png">
