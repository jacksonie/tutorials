#  3. Configuração do MariaDB

###  3.1 Configuração padrão

A configuração padrão do MariaDB normalmente usa cerca de 400 MB de RAM.

Existem muitos limites definidos que impedem a computação em RAM e
muitas consultas ao banco de dados resultam na criação de um 'arquivo temporário em disco' -
o que deixa o MariaDB muito lento.
Além disso, com a configuração padrão, os dados geralmente são carregados do disco.

Se você tiver alguma RAM sobressalente em seu servidor, vale a pena mudar.

###  3.2 Otimização - configuração de edição

Otimizar uma configuração do MariaDB é muito complexo.
Abaixo está um exemplo de configuração desenvolvido para grandes OTSs.

__CUIDADO!__
Se você deseja otimizar o banco de dados de um OTS já em operação,
então volte primeiro!

A configuração do MariaDB no Ubuntu 20.04 está no arquivo:
``
/etc/mysql/mariadb.conf.d/50-server.cnf
``
Exemplo de alterações de configuração após as quais o MariaDB deve usar 4-8 GB de RAM.
Recomendado para qualquer servidor com 16 GB de RAM e mais.

####  3.2.1 Alterando os Limites Básicos

Achar:
``
#key_buffer_size = 16 milhões
#max_allowed_packet = 16M
#thread_stack = 192K
#thread_cache_size = 8
``
Descomente e substitua os valores por:
``
key_buffer_size = 512 milhões
max_allowed_packet = 512M
thread_stack = 1M
thread_cache_size = 128
``

####  3.2.2 Alterar limites para conexões e tabelas abertas

Definimos valores altos que o servidor nunca deve atingir.

Achar:
``
#max_connections = 100
#table_cache = 64
``
Descomente e substitua os valores por:
``
max_connections = 50.000
table_cache = 50000
``

####  3.2.3 Alterando os limites das consultas processadas na RAM

Adicionar:
``
max_heap_table_size = 1G
tmp_table_size = 1G
bulk_insert_buffer_size = 1G
``

####  3.2.4 Mudando os limites do InnoDB

InnoDB é o mecanismo MariaDB que é usado pelo banco de dados OTSa.

Essas mudanças têm o maior impacto no tempo de gravação do OTSa.

Adicionar:
``
innodb_file_per_table = 1
innodb_buffer_pool_size = 4G
innodb_buffer_pool_instances = 8
innodb_log_file_size = 512M
innodb_log_buffer_size = 256M
innodb_flush_log_at_trx_commit = 2
``

####  3.2.5 Modo SQL - seg. fabricantes

Ace. fabricantes, como Gesior2012, não seguem o padrão SQL.
Por muitos anos não foi um problema
porque as configurações padrão permitiam quebrar algumas regras SQL.
Isso mudou no novo MySQL e MariaDB.

Felizmente, você pode mudar o modo de operação para o mesmo que era antes,
adicionando uma linha na configuração:
``
sql_mode = ''
``
####  3.2.6 Reinicie o MariaDB

Você deve reiniciar o MariaDB para que as alterações de configuração tenham efeito:
``
systemctl reiniciar mysql
``
(sim, escrevemos `mysql` e MariaDB reinicia)

###  3.3 Acesso ao banco de dados

Por padrão, MariaDB é acessível a partir do console com o usuário `root` sem senha.
Precisamos alterar o acesso ao usuário para que possamos fazer login com a senha.

No exemplo, definiremos a senha do usuário ` root` para ` secretpass` .

####  3.3.1 Login com senha

Acender:
``
mysql
``
O console do servidor MariaDB travou.

Fogo por sua vez:
1. Selecione o banco de dados que contém os usuários:
``
usar mysql;
``
2. Altere o tipo de login de `unix_socket`
(login baseado no usuário do console)
para entrar com uma senha:
``
UPDATE `user` SET` plugin` = 'mysql_native_password' WHERE `user` = 'root' AND` host` = 'localhost';
``
3. Defina a senha do usuário `root` para ` secretpass` :
``
SET PASSWORD FOR 'root' @ 'localhost' = PASSWORD ('secretpass');
``
4. Recarregue as configurações de usuário do MariaDB:
``
privilégios de descarga;
``
5. Desative os consoles MariaDB (você também pode clicar em CTRL + D)
``
saída
``

####  3.3.2 Login automático do console

Depois de definir uma senha, para fazer login no MariaDB a partir do console, você precisa digitar:
``
mysql -p
``
e, em seguida, digite a senha.

A senha pode ser inserida na configuração do MariaDB.
Com esta mudança, os comandos `mysql` e ` mysqldump` não exigirão mais uma senha .

Edite o arquivo:
``
/etc/mysql/mariadb.conf.d/50-client.cnf
``
Debaixo:
``
[cliente]
``
Adicione uma linha:
``
senha = senha secreta
``
Após esta alteração, é possível usar `mysql` ( sem` -p` ) novamente e não digitar uma senha.
