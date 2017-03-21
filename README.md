
# base_scripts
```
Scripts shell e sql
Dump_Banco:
pg_dump.exe --host localhost --port 5432 --username postgres --format tar --file C:\Users\patricia_mello\Documents\GI2S_DIM_Caucaia.backup GI2S_DIM

Restore:
pg_restore -i -h localhost -p 5432 -U postgres -d GI2S_OPE -v "C:\Users\patricia_mello\Documents\GI2S_OPE_CAucaia.backup"

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
```
