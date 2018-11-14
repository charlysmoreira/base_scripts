
# base_scripts
```

## Instalação do Java x.x.x.x

# Instalação do sdkman
curl -s "https://get.sdkman.io" | bash
# Vericar versao
java -version
# Listar os jdks
sdk list java
# Instalçao da versao desejada
sdk install java 7u141-zulu


Scripts shell e sql
Dump_Banco:
pg_dump --host localhost --port 5432 --username postgres --format tar --file /home/GI2S_APP_20170420.backup GI2S_APP

Restore:
pg_restore -i -h localhost -p 5432 -U postgres -d GI2S_APP -v /home/GI2S_APP_20170420.backup
Ou:
psql -U postgres -f GI2S_APP_20170703.sql GI2S_APP

Copiar do servidor para a pasta local
scp root@172.24.177.3:/home/GI2S_APP_20170420.backup .

Copia os dados de uma tabela para outro banco
pg_dump -h localhost -U postgres -t schema_version GI2S_APP | psql -U postgres -h 192.168.2.199 GI2S_APP

Matar as sessoes do usuarios no postgres
SELECT pg_terminate_backend(pid) FROM pg_stat_activity 
WHERE -- don't kill my own connection!
    pid <> pg_backend_pid()
    -- don't kill the connections to other databases
    AND datname = 'GI2S_APP';

Fazer Tunelamento:
ssh -f user@ip -L [porta_maquina_local]:localhost:[porta_maquina_servidor] -N
Ex: ssh -f root@www.gissa.com.br -L 28087:localhost:18087 -N

Instalando o java 
#!/bin/sh

echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
add-apt-repository ppa:webupd8team/java -y
apt update
apt install oracle-java8-installer -y
echo "Setting environment variables for Java 8.."
apt install oracle-java8-set-default -y

Cofiguracao do Https

#!/bin/bash

apt update -y
apt install apache2 apache2-dev libxml2-dev -y

echo "Criando diretorio de instalação do apache"
a2enmod xml2enc
a2enmod ssl
a2enmod proxy
a2enmod proxy_http
a2enmod proxy_ajp
a2enmod proxy_connect
a2enmod proxy_html
a2enmod proxy_wstunnel
a2enmod rewrite
a2enmod deflate
a2enmod headers

echo "Recarregando as configuraçoes do apache"
service apache2 restart

echo "Copiando arquivo de configuração"
cp apache/gissa.conf /etc/apache2/sites-available

CWD=$PWD

mkdir /home/gissa/www
mkdir /home/gissa/www/__static__
mkdir /home/gissa/www/errors

cp apache/errors/* /home/gissa/www/errors

chown -R gissa:gissa /home/gissa/www
chown -R gissa:gissa /home/gissa/www/*

echo "Criando pasta com arquivos de configuração ssl"
mkdir /etc/apache2/ssl
cd /etc/apache2/ssl
echo "Gerando chaves para o ssl"
openssl genrsa -out gissa.key 1024
openssl req -new -key gissa.key -x509 -out gissa.crt -subj '/C=BR/ST=Ceara/L=Fortaleza/O=Instituto Atlantico/OU=Atlantico/CN=Atlantico'

echo "Ativando o site"
cd /etc/apache/sites-available/
a2ensite gissa.conf

cd $CWD

echo "Instalando o Letsencrypt"
apt install python-letsencrypt-apache -y
letsencrypt --apache -d www.gissa.com.br -m contato@gissa.com.br --agree-tos --redirect

echo "Reiniciando o apache"
service apache2 restart

Permissao para usuario
chown -R user_novo:user_novo /home/user_novo/novo_dir?teste.txt

Ler saida de console
tail -f output.log

* Copiar tabelas
pg_dump  -U postgres -F custom -v -f backup_tabela_licitacao.backup -t 'ctrlint_tcm_licitacoes' -d GI2S_OPE_230440

* Criar Banco de dados
createdb --host localhost --port 5432 --username postgres GI2S_STG_231290
* Delete Banco de Dados
dropdb --host localhost --port 5432 --username postgres GI2S_DIM_230440

http://www.springboottutorial.com/creating-rest-service-with-spring-boot


ssh root@www.gissa.com.br -L 28087:localhost:18087 -L 5433:gissadb:5432


===============================

* Crie um usuário gissa

* Crie uma pasta .gissa

* Crie uma pasta robo e work dentro de .gissa

* Copie o robo-etl.jar para a pasta robo

* Copie o arquivo application.yml para a pasta robo. Exemplo do conteúdo:

```
xmpp:
  username: nome_do_robo_no_xmpp
  password: senha_do_robo_no_xmpp
  host: xmpp.gissa.com.br
  port: 443
  serviceName: xmpp.gissa.com.br
  useHttps: true
  filePath: /websocket/
  resource: gissa
  rooms: gissa

```

* Copie o arquivo robo.env para a pasta robo com o conteúdo abaixo:

```
JAVA_OPTS="-Xms1024m -Xmx2048m -XX:+UseParallelGC -XX:+AggressiveOpts -XX:+UseFastAccessorMethods"
```

* Criando serviço

** Entre no diretório /etc/systemd/system ** Crie um arquivo .service Ex: robo.service e adicione o conteúdo abaixo

```
[Unit]
Description=Gissa Robo

[Service]
User=gissa
Type=oneshot
ExecStart=/home/gissa/.gissa/robo/robo-etl.jar start
ExecStop=/home/gissa/.gissa/robo/robo-etl.jar stop
RemainAfterExit=yes
EnvironmentFile=/home/gissa/.gissa/robo/robo.env

[Install]
WantedBy=multi-user.target
After=network-online.target

```

** Entre no diretório /etc/systemd/system ** Crie um arquivo .service Ex: robo.service e adicione o conteúdo abaixo

```
[Unit]
Description=Gissa Robo

[Service]
User=gissa
ExecStart=/home/gissa/.gissa/robo/robo-etl.jar
EnvironmentFile=/home/gissa/.gissa/robo/robo.env
SuccessExitStatus=143
Restart=always

[Install]
WantedBy=multi-user.target
After=network-online.target

```

* Alguns comandos para trabalhar com o serviço

```
systemctl disable robo.service
systemctl daemon-reload
systemctl enable robo.service
systemctl start robo.service
systemctl stop robo.service
systemctl status robo.service
```















```
