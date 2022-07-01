#  8. Configure o limite de arquivos abertos

No Linux, as conexões abertas (entre o player e o servidor) contam como arquivos abertos.
O limite de arquivo aberto padrão é 1024.
Basta alguém abrir 1024 conexões com o servidor do jogo/servidor web e
ninguém mais poderá entrar/abrir a página.

Eventualmente, seu servidor se tornará super popular e
de repente, em 1000 online, o servidor offline começará a voltar quando alguém tentar fazer login.

O número de arquivos abertos por um determinado aplicativo é limitado por 3 configurações:
-  `/etc/security/limits.conf` - limita o número de arquivos abertos por um determinado usuário Linux
-  `/ etc / systemd` - limita o número de arquivos abertos por determinado aplicativo
- também há um limite geral para arquivos abertos no sistema, mas no novo Linux é alto por padrão

Estou aumentando o limite para __300000__ .
A RAM mais rápida acabará / a CPU será carregada em 100%,
que qualquer um dos aplicativos atinge esse número de conexões.

##  8.1 Editando o limite de arquivos abertos do usuário

Estamos editando o limite do número de arquivos abertos de 3 usuários:
-  `root` - você executa o OTS a partir dele
-  `mysql` - MariaDB está rodando neste usuário
-  `www-data` - este usuário está executando ` nginx` e `php`

Se você executar o OTS de um usuário diferente de `root` , defina limites altos para esse usuário também.

Os limites são definidos no arquivo:
```
/etc/security/limits.conf
```

No final do arquivo, adicione:
```
mysql hard nofile 300000
mysql soft nofile 150000
www-data hard nofile 300000
www-data soft nofile 150000
root hard nofile 300000
root soft nofile 150000
```
( `nofile` é a abreviação de ` número de arquivos` )

##  8.2 Editando o limite de arquivos abertos por determinado aplicativo

Cada aplicação tem sua configuração em algum lugar em `/ etc/systemd` .
Você tem que encontrar e editar os arquivos de configuração para um determinado aplicativo

###  8.2.1 Editar limites de aplicativos nginx

Abra para encontrar os arquivos de configuração `nginx` :
```
encontre /etc/systemd -name 'nginx*'
```
Abra cada arquivo encontrado.

Nele, encontre uma linha que comece com:
```
LimitNOFILE=
```
Se estiver, substitua-o por:
```
LimitNOFILE=300000
```
Se não, encontre:
```
[Serviço]
```
e adicione uma linha abaixo:
```
LimitNOFILE=300000
```

###  8.2.2 Faça o mesmo para PHP e MariaDB

Você pode encontrar a configuração do PHP executando:
```
encontre /etc/systemd -name 'php*'
```
Você pode encontrar a configuração do MariaDB executando:
```
encontre /etc/systemd -name 'mariadb*'
```
###  8.2.3 Reinicie o servidor

Você pode combinar e recarregar todas as permissões alteradas com comandos,
mas é muito mais fácil fazer isso reiniciando o Linux:
```
reinício
```
