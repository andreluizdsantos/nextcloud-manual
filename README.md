# nextcloud-manual
Instalação Manual Nextcloud + Ubuntu 22.04

- Passo 1 – Instale o PHP e o Apache Web Server
- Passo 2 – Instale o servidor de banco de dados MySQL / MariaDB
- Passo 3 – Baixe e instale o Nextcloud
- Etapa 4 – Configurar o Apache para servir o Nextcloud
- Passo 5 – Habilite o modo ReWrite e reinicie o servidor
- Etapa 6 – Concluir a instalação do Nextcloud via GUI

# Passo 1 – Instale o PHP e o Apache Web Server

Antes de começar atualize o sistema.
```bash
sudo apt update
sudo apt upgrade
sudo reboot
```
Execute o seguinte comando para instalar o servidor web Apache e PHP:
```bash
sudo apt install -y php-cli php-fpm php-json php-intl php-imagick php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath apache2 libapache2- mod-php php-gmp
```
Quando a instalação dos pacotes estiver concluída, defina as variáveis do **PHP** usando o seguinte comando:
```bash
sudo vim /etc/php/*/apache2/php.ini
```
Em seguida, defina as variáveis **PHP;** é como segue:
```bash
date.timezone = America/Sao_Paulo
memory_limit = 512M
upload_max_filesize = 500 milhões
post_max_size = 500 milhões
max_execution_time = 300
```
E reinicie o servidor web apache usando o seguinte comando:
```bash
sudo systemctl reiniciar apache2
```
# Passo 2 – Instale o servidor de banco de dados MySQL / MariaDB
Em seguida, execute o seguinte comando para instalar o servidor de banco de dados MariaDB ou MySQL:
```bash
sudo apt -y install mariadb-server
```
Proteja o servidor de banco de dados MariaDB usando o seguinte comando:
```bash
sudo mysql_secure_installation
```
Em seguida, execute o seguinte comando para criar o banco de dados:
```bash
sudo mysql -u root -p
```
```mysql
CREATE DATABASE nextcloud;
CREATE USER 'administrador'@'localhost' IDENTIFIED BY 'SenhaForte123@';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'administrador'@'localhost';
FLUSH PRIVILEGES;
Quit;
```
Não se esqueça de substituir *SenhaForte123@* pela senha de usuário do banco de dados.
# Passo 3 – Baixe e instale o Nextcloud
Execute os seguintes comandos para baixar e instalar o Nextcloud no linux ubuntu:
```bash
sudo apt install -y wget unzip
wget https://download.nextcloud.com/server/releases/latest.zip
```
Depois que o arquivo for baixado, extraia-o usando o seguinte comando:
```bash
unzip latest.zip
```
Mova a pasta resultante para /srv
```bash
sudo mv nextcloud/ /srv
```
Usando o seguinte comando para alterar as permissões de diretório para o  www-datausuário:
```bash
sudo chown -R www-data:www-data /srv/nextcloud/
```
# Etapa 4 – Configurar o Apache para servir o Nextcloud
Em seguida, execute o seguinte comando na linha de comando para criar um arquivo VirtualHost para Nextcloud
```bash
sudo vim /etc/apache2/conf-enabled/nextcloud.conf
```
Depois disso, adicione o seguinte conteúdo ao arquivo:
```conf
<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /srv/nextcloud/
     ServerName example.com
     ServerAlias ​​www.example.com
     ErrorLog /var/log/apache2/nextcloud-error.log
     CustomLog /var/log/apache2/nextcloud-access.log combinado
 
    <Diretório /srv/nextcloud/>
	Opções + SeguirSymlinks
	Permitir substituir tudo
        Exigir todos os concedidos
 	SetEnv HOME /srv/nextcloud
 	SetEnv HTTP_HOME /srv/nextcloud
 	<IfModule mod_dav.c>
  	  Davi desligado
        </IfModule>
    </Diretório>
</VirtualHost>
```
