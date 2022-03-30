## *Si vous utilisez une machine non-graphique suivez [ce](https://github.com/TheJordanDev/Tutoriels/blob/main/Installer%20SSH.md) tutoriel pour utiliser VSCode*

## *Avant de commencer il faut installer PHP 8, utilisez [ce](https://github.com/TheJordanDev/Tutoriels/blob/main/Installer%20PHP8.md) tutoriel*

---

## 🔴Vous devez utiliser votre compte utilisateur (pas root) pour suivre ce tutoriel🔴

> Déplacer composer dans /bin/ pour y acceder partout

```shell
cd ~
mkdir site && cd "$_"
```

> Aller dans le dossier où vous voulez votre site

```shell
composer create-project laravel/laravel:^9.0 example-app
cd ~/site/<nom du projet>/
```

> Si il y a une erreur, installez les dependances requises

# 🔴IMPORTANT🔴

> Si vous avez une machine graphique, executez la commande suivante avec le terminal interne de la machine.
> 
> Si vous utilisez une machine non-graphique, utilisez l'extension "Remote SSH" de VSCode et connectez vous avec ssh, puis utilisez le terminal de VSCode pour executer la commande suivante, le site sera accessible depuis votre navigateur.

```shell
php artisan serve
```

> Le site sera servi à l'adresse 127.0.0.1:8000 *Pour l'arrêter faites CTRL+C*

---

## Installer la debug bar

> Quand l'installation est terminée, installez la debug bar (pensez à éteindre la site avant d'installer la dépendance)

```shell
composer require barryvdh/laravel-debugbar --dev
```

> Puis changez la valeur de APP_DEBUG dans le fichier .env en true
> 
> *si APP_ENV vaut "local" le site est en mode dev*

```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost
```
