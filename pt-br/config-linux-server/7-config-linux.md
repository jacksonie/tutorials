#  7. Configuração do Linux

É uma boa ideia reorganizar algumas opções na configuração do sistema para bloquear ataques básicos.

Além disso, você pode alterar as configurações de rede,
para que o servidor pudesse lidar com mais conexões no caso de um ataque DDoS.

##  7.1 Editando a configuração

A configuração das configurações básicas do Linux está no arquivo:
``
/etc/sysctl.conf
``

###  7.1.1 Proteção contra falsificação

Encontre a linha com `Spoof protection` e descomente a configuração abaixo dela, ou seja:
``
# net.ipv4.conf.default.rp_filter = 1
# net.ipv4.conf.all.rp_filter = 1
``
Mudar para:
``
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.all.rp_filter = 1
``

###  7.1.2 Ataques MITM

Encontre a linha com `MITM attack` e descomente a configuração abaixo dela, ou seja:
``
# net.ipv4.conf.all.accept_redirects = 0
# net.ipv6.conf.all.accept_redirects = 0
``
Mudar para:
``
net.ipv4.conf.all.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
``

###  7.1.3 não somos um roteador

Encontre as linhas com `we are not a router` (são 2) e descomente as configurações abaixo delas, ou seja:
``
# net.ipv4.conf.all.send_redirects = 0
``
Mudar para:
``
net.ipv4.conf.all.send_redirects = 0
``
e:
``
# net.ipv4.conf.all.accept_source_route = 0
# net.ipv6.conf.all.accept_source_route = 0
``
Mudar para:
``
net.ipv4.conf.all.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0
``

###  7.1.4 Configurações de rede

Apagamento mais rápido de 'conexões fechadas', reutilizando as mesmas portas e
limites mais altos para conexões/pacotes esperando para serem servidos.

No final do arquivo, adicione:
``
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10.000 65.000
net.ipv4.tcp_max_syn_backlog = 8192
net.core.netdev_max_backlog = 100.000
``

###  7.1.5 Carregando alterações

Para carregar as alterações de `/ etc / sysctl.conf` sem reiniciar o Linux, execute:
``
sysctl -p
``
