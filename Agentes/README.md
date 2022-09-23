<h1 Align="center">Teste de comunicação Zabbix</h1>

Senhores, 

Abaixo segue um site e instruções de validação de comunicação:

<bloq
http://portquiz.net/ 
Ele tem todas portas TCP abertas, então conseguimos testar, por exemplo, a 10050, que o Zabbix usa. 

<ul>
<li>Portanto, quando não estiver comunicando, testaremos 4 coisas: </li>
</ul>
<ol>
<li>Tracert para o Proxy/Server (Preferencialmente, a partir de mais de uma máquina e do proxy para o server, se o cliente tiver proxy dedicado) </li>
  <pre><code>sudo tracert 127.0.0.1 </pre></code>
  <li>Tracert para o Google.com (Mesmo esquema de cima) </li>
   <pre><code>sudo tracert 8.8.8.8 </pre></code>
<li>Telnet para o Proxy/Server na porta 10050 (Mesmo esquema de cima, a partir de mais de uma máquina e do proxy para o server) </li>
 <pre><code> sudo telnet 127.0.0.1 10050 or 10051</pre></code><br>
Telnet para o http://portquiz.net/ na porta 80 e na porta 10050 (80 é WEB padrão, o mais básico a liberar). <br>
Telnet para o Google.com na porta 80.

  <h3>Orientações</h3>
Se os primeiros testes funcionarem, ele chegar ao IP de destino, quer dizer que tem comunicação.<br>
Se o segundo funcionar, o Zabbix deveria estar funcionando, o problema é em outro lugar.<br>

Se o primeiro falhar, nem precisa fazer o segundo, a comunicação não está nem saindo. <br>
Se o primeiro funcionar e o segundo falhar, tem comunicação mas a portas estão bloqueadas, o cliente tem que verificar suas regras.


Qualquer duvida ou adição, só me informar.
