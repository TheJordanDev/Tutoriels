## Vous devez être en super-utilisateur (root) pour suivre ce tutoriel

---

# Télécharger PHP

> Il faut d'abord ajouter une source de repository contenant la version 8 de PHP

```shell
apt update -y
apt -y upgrade
reboot
apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/sury-php.list
wget -qO - https://packages.sury.org/php/apt.gpg | apt-key add -
apt update -y
```

```shell
apt install php8.1 -y
apt install php8.1-{mysql,cli,common,imap,ldap,xml,fpm,curl,mbstring,zip} unzip -y
```

> Installer composer *si l'instalation ne marche pas car "Installer corrupt" n'exécutez juste pas la deuxime ligne est tout ira bien*

```shell
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
mv composer.phar /usr/local/bin/composer
```
