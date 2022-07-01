#  4. Configuração do nginx

###  4.1 Configuração padrão

A configuração padrão do nginx é aproximadamente OK.

As mudanças que faremos:
- adicionaremos um formato de login que salva informações adicionais (país do usuário e tempo de execução do PHP)
- cache de arquivos de log
- cache de arquivo aberto
- tamanho máximo maior do arquivo carregado
- desconexão mais rápida das conexões

O país do usuário só será logado após adicionar CloudFlare.

###  4.2 Editando a configuração

A configuração do nginx no Ubuntu 20.04 está no arquivo:
``
/etc/nginx/nginx.conf
``

###  4.2.1 Alterando as configurações
Achar:
``
http{
``
Adicione a linha abaixo:
``
        log_format cloudflarelog '$ remote_addr "$ HTTP_CF_IPCOUNTRY" "$ http_host" [$ time_local]'
        '"$ solicitação" $ status $ body_bytes_sent "$ upstream_response_time" "$ http_referer" "$ http_user_agent"';
        open_log_file_cache max = 1000 inativo = 20s válido = 1m min_uses = 2;
        open_file_cache max = 200000 inativo = 20s;
        open_file_cache_valid 30s;
        open_file_cache_min_uses 2;
        open_file_cache_errors ativado;
        client_max_body_size 128M;
``

Achar:
``
keepalive_timeout 65;
``
Mudar para:
``
keepalive_timeout 5;
``

###  4.2.2 Reinicie o nginx

Você deve reiniciar o nginx para que as alterações de configuração tenham efeito:
``
systemctl reinicie o nginx
``
