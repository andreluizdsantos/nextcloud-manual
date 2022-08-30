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
Execute o seguinte comando na linha de comando para instalar o servidor web Apache e PHP:
```bash
sudo apt install -y php-cli php-fpm php-json php-intl php-imagick php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath apache2 libapache2- mod-php
```

```bash
sudo vim /etc/php/*/apache2/php.ini
```
