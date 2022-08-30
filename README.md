# Instalação Manual Nextcloud no Ubuntu 22.04

- Passo 1 – Instale o PHP e o Apache Web Server
- Passo 2 – Instale o servidor de banco de dados MySQL / MariaDB
- Passo 3 – Baixe e instale o Nextcloud
- Etapa 4 – Configurar o Apache para servir o Nextcloud
- Passo 5 – Habilite o modo ReWrite e reinicie o servidor
- Etapa 6 – Concluir a instalação do Nextcloud via GUI
- Etapa 7 - Ajustar configurações config.php
- Etapa 8 - Proteger e configurar "servidor mariadb".
- Etapa 9 - Outros ajustes pós instalação

## Passo 1 – Instale o PHP e o Apache Web Server

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
## Passo 2 – Instale o servidor de banco de dados MySQL / MariaDB
Em seguida, execute o seguinte comando para instalar o servidor de banco de dados MariaDB ou MySQL:
```bash
sudo apt -y install mariadb-server
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
## Passo 3 – Baixe e instale o Nextcloud
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
sudo mkdir /srv
sudo mv nextcloud/ /srv
```
Usando o seguinte comando para alterar as permissões de diretório para o  www-datausuário:
```bash
sudo chown -R www-data:www-data /srv/nextcloud/
```
## Etapa 4 – Configurar o Apache para servir o Nextcloud
Em seguida, execute o seguinte comando na linha de comando para criar um arquivo VirtualHost para Nextcloud
```bash
sudo vim /etc/apache2/conf-enabled/nextcloud.conf
```
Depois disso, adicione o seguinte conteúdo ao arquivo:
```bash
<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /srv/nextcloud/
     ServerName example.com
     ServerAlias www.example.com
     ErrorLog /var/log/apache2/nextcloud-error.log
     CustomLog /var/log/apache2/nextcloud-access.log combined
 
    <Directory /srv/nextcloud/>
	Options +FollowSymlinks
	AllowOverride All
        Require all granted
 	SetEnv HOME /srv/nextcloud
 	SetEnv HTTP_HOME /srv/nextcloud
 	<IfModule mod_dav.c>
  	  Dav off
        </IfModule>
    </Directory>
</VirtualHost>
```
## Passo 5 – Habilite o modo ReWrite e reinicie o servidor
Usando o seguinte comando para habilitar os módulos Apache necessários e reiniciar o serviço:
```bash
sudo a2enmod rewrite dir mime env headers

sudo systemctl restart apache2
```
## Etapa 6 – Concluir a instalação do Nextcloud via GUI
Abra seu navegador e aponte para o seguinte endereço:
```HTML
http://IP_DO_SERVIDOR/nextcloud/ 
ou
http://ENDEREÇO_DO_SERVIDOR/nextcloud/
```
Depois que o assistente de instalação for carregado, crie uma conta de usuário administrador do nextcloud. Digite o nome de usuário e senha. Além disso, clique no  link **Armazenamento e Banco de Dados** para acessar opções de configuração de instalação adicionais para o diretório de dados e banco de dados do Nextcloud.

Em seguida, preencha os detalhes da conexão do banco de dados e clique em **Concluir configuração**.

Assistente de configuração do Nextcloud
Quando a instalação estiver concluída, veremos a seguinte janela. Clique na seta para a frente que aparecerá no lado direito da janela azul para prosseguir e siga as instruções.
## Etapa 7 - Ajustar configurações config.php
Revisar arquivo config.php
```bash
sudo vim /srv/nextcloud/config/config.php
```
```php
<?php
$CONFIG = array (
  'instanceid' => “xxxxxxxxxxxxxxxx”, # Aqui você verá uma ID unica
  'passwordsalt' => “xxxxxxxxxxxxxxx”,
  'secret' => “xxxxxxxxxxxxxxxxxxxxxx”,
  'trusted_domains' =>
array (
    0 => 'localhost',
    1 => 'exemplo.com', # - confira o domínio aqui
  ),
  'datadirectory' => '/home/nextclouddata', # Confira o caminho da pasta DATA aqui (se você trocou)
  'dbtype' => 'mysql',
  'version' => 'XX.X.X.XX', # Versão do Nextcloud
  'overwrite.cli.url' => 'http://exemplo.com/', # confira o domínio aqui
  'dbname' => 'DBnextcloud',
  'dbhost' => 'localhost',
  'dbport' => '',
  'dbtableprefix' => 'oc_',
  'mysql.utf8mb4' => true, # Suporte a acentuação
  'dbuser' => 'DBusername', # usuário nextcloud
  'dbpassword' => 'DBpassword', # senha padrão
  'installed' => true,
  'overwriteprotocol' =>'https', # nextcloud precisa de uma conexão segura (= https) para usar __Host-cookies.
  'default_locale' => 'pt_BR', # Define Local padrão Brasil
  'default_language' => 'pt_BR', # Define Linguagem padrão Brasil
  'default_phone_region' => 'BR', # Define Região padrão Brasil
);
```
## Etapa 8 - Depois de acessar e configurar o Nextcloud é necessário proteger e configurar nosso "servidor mariadb".
No terminal, esses comandos iniciarão as configurações e o script fará algumas perguntas, você pode responder "sim" "Y" a todas as perguntas e também criar um novo usuário raiz do banco de dados:
```bash
sudo mysql_secure_installation
Enter current password for root (enter se não tiver):    pressione Enter
Set root password? [Y/n]: Y
New password:   # Digite a senha (Esta é a senha de root do banco MariaDB esta PRECISA ser diferente da senha root do sistema)
Re-enter new password: # Repita a senha
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]: Y
Reload privilege tables now? [Y/n]: Y   
```
Reinicie o MariaDB
```bash
sudo systemctl restart mariadb.service
```
## Etapa 9 - Outros ajustes pós instalação
Alterar o arquivo nextcloud.conf comentando o Alias para remover alerta de __Host-Prefix
```bash
sudo vim /etc/apache2/sites-available/nextcloud.conf
```
```bash
#Alias /nextcloud "/var/www/nextcloud/"
```
Removendo alguns alertas da informação da instalação
```bash
apt install -y libmagickcore-6.q16-6-extra imagemagick
```

## Conclusão
Através deste tutorial, aprendemos como instalar e configurar o Nextcloud no Linux ubuntu 22.04.

Fonte: [Tutsmake - Como instalar Nextcloud no Ubuntu 22.04](https://www.tutsmake.com/how-to-install-nextcloud-on-ubuntu-22-04/)

[Nextcloud](https://github.com/docker-library/docs/blob/master/nextcloud/README.md)
