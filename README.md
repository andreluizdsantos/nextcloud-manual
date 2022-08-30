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
sudo apt install -y php-cli php-fpm php-json php-intl php-imagick php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath apache2 libapache2- mod-php
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
