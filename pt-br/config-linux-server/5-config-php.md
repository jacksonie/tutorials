#  5. Configuração do PHP

###  5.1 Configuração padrão

A configuração padrão do PHP é aproximadamente OK.

As mudanças que faremos:
- aumentaremos o limite de RAM para uma única consulta
- aumentaremos o limite de tamanho dos arquivos enviados

###  5.2 Editando a configuração

A configuração do PHP no Ubuntu 20.04 está no arquivo:
```
/etc/php/7.4/fpm/php.ini
```

###  5.2.1 Alterar o limite de RAM
Achar:
```
memory_limit = 128M
```
Mudar para:
```
memory_limit = 512M
```

###  5.2.2 Alterar o limite de arquivos transferidos
Achar:
```
post_max_size = 8M
```
Mudar para:
```
post_max_size = 128 milhões
```

Achar:
```
upload_max_filesize = 2M
```
Mudar para:
```
upload_max_filesize = 128 milhões
```

###  5.2.3 Reinicie o PHP

Para que as alterações de configuração tenham efeito, o PHP deve ser reiniciado:
```
systemctl reinicie php7.4-fpm
```
