#  2. Instalando nginx, MariaDB, PHP e ferramentas básicas

###  2.1 Instalando o PHP

Instalando PHP ( `php-fpm` for` nginx` ) e bibliotecas PHP para Gesior2012:
```
apt install php-fpm php-bcmath php-common php-curl php-json php-gd php-mbstring php-mysql php-pdo php-xml php-zip
```
Em sistemas com versões do PHP anteriores a 8.0, você ainda precisa instalar o `php-json` :
```
apt instalar php-json
```
A partir da versão 8.0 , o `json` está embutido no PHP e não precisa ser instalado separadamente.

###  2.2 Instalando o nginx

Instalando o nginx:
```
apt instalar nginx
```

###  2.3 Instalando o MariaDB

MariaDB é um substituto para o MySQL. Em muitos lugares no Linux, as configurações do MariaDB têm `mysql` no nome .

Instalação do MariaDB:
```
apt instalar mariadb-server mariadb-client
```

###  2.4 Ferramentas básicas

É uma boa idéia instalar algumas ferramentas básicas.
Eles são úteis no diagnóstico de problemas do servidor e durante ataques.
```
apt install fail2ban iptables-persistent htop nload tcpdump mc zip unzip git screen screenie net-tools moreutils traceroute
```
Durante a instalação, 2 questões podem surgir em relação à notação `iptables` . Nós respondemos 'não' a ​​ambos.

O que instalamos:
-  `fail2ban` - protege SSH, bloqueia IP após várias tentativas de login com senha errada
-  `iptables-persistent` - permite que você salve as regras do `iptables` em um arquivo e as carregue após a reinicialização do servidor
-  `htop` - versão estendida do ` top`
-  `nload` - mostra o uso atual do link no servidor
-  `tcpdump` - salva/exibe todos os pacotes de rede de entrada/saída
-  `zip` + ` unzip` - programas para empacotar / extrair arquivos `.zip`
-  `git` - cliente GIT
-  `mc` - gerenciador gráfico de arquivos rodando no console
-  `screen` + ` screenie` - aplicativo para gerenciar a `tela` de trabalho
-  `net-tools` - contém` netstat` que permite verificar alguns IPs e portas que o servidor está escutando
-  `moreutils` - contém` ts` , um aplicativo que permite adicionar tempo em milissegundos aos logs (por exemplo, do console OTS)
-  `traceroute` - permite rastrear a rota dos pacotes de rede (diagnóstico de problemas de link)
