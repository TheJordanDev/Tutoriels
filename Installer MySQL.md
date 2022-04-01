> Pour installer MySQL il faut d'abord télécharger le fichier d'installation de MySQL.

```shell
wget http://repo.mysql.com/mysql-apt-config_0.8.13-1_all.deb
```

> Puis l'installer

```shell
apt install ./mysql-apt-config_0.8.13-1_all.deb
```

![Image1](https://raw.githubusercontent.com/TheJordanDev/Tutoriels/main/assets/mysql/image1.png)

![](https://raw.githubusercontent.com/TheJordanDev/Tutoriels/main/assets/mysql/image2.png)

> Choisissez la version mysql-5.7 (une version recente qui marche bien)

![](https://raw.githubusercontent.com/TheJordanDev/Tutoriels/main/assets/mysql/image3.png)

![](https://raw.githubusercontent.com/TheJordanDev/Tutoriels/main/assets/mysql/image4.png)

> Après un peu de téléchargement vous allez revoir ce menu, refaites OK

> Il faut maintenant mettre à jour la machine (avant il faut définir une clé pour le téléchargement)

```shell
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 467B942D3A79BD29
apt update
```

> Vous pouvez maintenant installer mysql-server

```shell
apt install -y mysql-server
```

> Vous allez devoir choisir un mot de passe pour l'utilisateur <u>root </u>pour MySQL. (Il faut l'entrer 2 fois)

![](https://raw.githubusercontent.com/TheJordanDev/Tutoriels/main/assets/mysql/image5.png)

> Pour vous connecter en <u>root</u> il faut faire.

```shell
mysql -u root -p
```

> Et entrez le mot de passe pour root
