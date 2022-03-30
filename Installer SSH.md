## 🔴Vous devez être en super-utilisateur (root) pour suivre ce tutoriel🔴

---

> Si vous utilisez une machine non-graphique/terminal, installez SSH et VSCode.
> 
> Vous en aurez besoin plus tard.

```shell
apt install ssh
```

> Et modifiez le fichier /etc/ssh/sshd_config

```
#PermitRootLogin prohibit-password
----------------------------------
PermitRootLogin yes
```

> Et redémarrez ssh

```shell
systemctl restart ssh
```

> Puis installez l'extension Remote SSH sur VSCode

![](C:\Users\thejo\AppData\Roaming\marktext\images\2022-03-18-13-47-19-image.png)

> Et connectez-vous à votre machine

<img src="file:///C:/Users/thejo/AppData/Roaming/marktext/images/2022-03-18-14-03-54-image.png" title="" alt="" width="599">

![](C:\Users\thejo\AppData\Roaming\marktext\images\2022-03-18-14-00-43-image.png)

> La commande à rentrer est la suivante

```shell
ssh <utilisateur>@<ip_machine>
```

> Vous verrez votre machine apparaître dans la liste (n°3)

<img title="" src="file:///C:/Users/thejo/AppData/Roaming/marktext/images/2022-03-18-14-06-07-image.png" alt="" data-align="center" width="359">

> Cliquez sur l'icone à droite pour vous connecter, puis suivez ce que vous dis VSCode
