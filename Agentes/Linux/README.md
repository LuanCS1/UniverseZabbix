<!DOCTYPE html>
<h1 align="center" ><b>Linux Install</b></h1>

<b><h4 align="center">NMS e NUA install LINUX</b></h4>

<pre><code>sudo rpm -Uvh /home/olos/Downloads/zabbix-agent-5.4.9-1.el7.x86_64.rpm
sudo rpm -Uvh /home/olos/Downloads/zabbix-agent-5.4.9-1.el6.x86_64.rpm
rpm -Uvh https://repo.zabbix.com/zabbix/4.4/rhel/6/x86_64/zabbix-agent-5.4.9-1.el6.x86_64.rpm
</pre></code>

<b><h4 align="center">BOT Install :</b></h4>

<pre><code>sudo rpm -Uvh /home/olos/downloads/zabbix-agent-5.4.9-1.el7.x86_64.rpm
sudo rpm -Uvh /home/olos/zabbix-agent-5.4.9-1.el7.x86_64.rpm</pre></code>

<b><h4 align="center">VPL install:</b></h4>

<pre><code>sudo dpkg -i /home/olos/Downloads/zabbix-agent_3.4.0-1+wheezy_amd64.deb
wget https://repo.zabbix.com/zabbix/3.4/debian/pool/main/z/zabbix/zabbix-agent_3.4.0-1%2Bwheezy_amd64.deb
sudo dpkg -i zabbix-release_3.0-1+wheezy_all.deb
sudo apt-get update
sudo apt-get install zabbix-agent</pre></code>

<b><h4 align="center">UBUNTU install: </b></h4>

<pre><code>wget https://repo.zabbix.com/zabbix/5.4/ubuntu-arm64/pool/main/z/zabbix/zabbix-agent_5.4.12-1+ubuntu20.04_arm64.deb
sudo dpkg -i zabbix-agent_5.4.12-1+ubuntu20.04_arm64.deb
sudo dpkg -i \home\olos/downloads/zabbix-agent-5.4.12-1+ubuntu20.04_arm64.deb
sudo dpkg -i \Downloads\zabbix-agent-5.4.12-1+ubuntu20.04_arm64.deb</pre></code>

<b><h4 align="center">UBUNTU install Focal:</b></h4>

<pre><code>wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix/zabbix-agent_5.0.26-1+focal_amd64.deb 
sudo dpkg -i /olos/zabbix-release_5.0-1+focal_all.deb
sudo dpkg -i zabbix-release_5.4-1+ubuntu20.04_all.deb
sudo dpkg -i zabbix-agent2_5.4.12-1+ubuntu20.04_amd64.deb

sudo dpkg -i zabbix-agent_5.0.23-1+focal_amd64.deb
sudo dpkg -i zabbix-agent_5.0.26-1+focal_amd64.deb

sudo dpkg -i \Downloads\zabbix-release_5.0-1+focal_all.deb
sudo dpkg -i \Downloads\zabbix-release_5.4-1+ubuntu20.04_all.deb</pre></code>

<b><h2 align="center"> LINUX CERTIFICADO:</b></h2>

<pre><code>TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=zabbix.com.br
TSLPSKFile=/etc/zabbix/zabbix_agentd.psk</pre></code>

<b><h4 align="center">Criar um arquivo no caminho /etc/zabbix/zabbix_agentd.psk e colocar a chave:</b></h4>
<pre><code><s>26a87124d0cdb6c7f1a9583167df718b9bebdf06dca5d45b5481aa98ad99aa59</s></pre></code>

<b><h4 align="center">Abrir Arquivo PSK</b></h4>
<pre><code>sudo nano /etc/zabbix/zabbix_agentd.psk</pre></code>

<b><h4 align="center">Abrir Arquivo Zabbix.Conf</b></h4>
<pre><code>sudo nano /etc/zabbix/zabbix_agentd.conf</pre></code>

<b><h4 align="center">Abrir Arquivo Zabbix.Conf - agent2</b></h4>
<pre><code>sudo nano /etc/zabbix/zabbix_agent2.conf</pre></code>

<b><h4 align="center">Somente em Servidores CentOS</b></h4>
O pacote do Agent2 pode ser desinstalado com o comando abaixo:
<pre><code>sudo rpm -e zabbix-agent2</pre></code>

Caso queira consultar os pacotes zabbix instalados, isso pode ser feito com o comando abaixo.
<pre><code>rpm -qa | grep Zabbix</pre></code>

<b><h4 align="center">Somente em Servidores Debian</b></h4>
O pacote do Agent2 pode ser desinstalado com o comando abaixo:
<pre><code>sudo dpkg -r zabbix-agent2
sudo apt-get purge zabbix-agent</pre></code>

Caso queira consultar os pacotes zabbix instalados, isso pode ser feito com o comando abaixo.
<pre><code>dpkg -l Zabbix</pre></code>

Caso queira consultar os pacotes zabbix instalados no Ubuntu, isso pode ser feito com o comando abaixo.
<pre><code>sudo dpkg -l | grep Zabbix</pre></code>

<b><h4 align="center">Ver a versão</b></h4>
<pre><code>cat /etc/*-release</pre></code>

<b><h4 align="center">Ver o Log</b></h4>
<pre><code>sudo tail /var/log/zabbix/zabbix_agentd.log
sudo tail -f /var/log/zabbix/zabbix_agentd.log
sudo tail -f /var/log/zabbix-agent/zabbix_agentd.log</pre></code>

<b><h4 align="center">Restart Automatico</b></h4>
<pre><code>chkconfig zabbix-agent on 
sudo systemctl enable zabbix-agent </pre></code>

<b><h4 align="center">Restart e verificação de log</b></h4>
<pre><code>sudo systemctl restart zabbix-agent && tail -f /var/log/zabbix/zabbix_agentd.log
sudo systemctl restart zabbix-agent2 && tail -f /var/log/zabbix/zabbix_agent2.log
sudo systemctl restart zabbix-proxy && tail -f /var/log/zabbix/zabbix_proxy.log</pre></code>
