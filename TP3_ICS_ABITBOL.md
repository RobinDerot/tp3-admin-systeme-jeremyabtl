3ICS - Jérémy ABITBOL 

# Compte rendu : TP3 - Gestion des paquets

## Exercice 1. Commandes de base

Commencez par mettre à jour votre système avec les commandes vues dans le cours. 

>sudo apt full-upgrade

Donnez les commandes répondant aux questions suivantes : 

1. Quels sont les 5 derniers paquets installés sur votre machine? 

Les fichiers d’historique (dpkg.log) se trouvent dans /var/log. Il faut donc se déplacer dans ce fichier via la commmande :
> cd /var/log

Ensuite on demande l'historique des 5 derniers paquets :
>tail -n 5 dpkg.log

```
2019-09-23 12:34:29 status installed systemd:amd64 240-6ubuntu5.7
2019-09-23 12:34:35 status installed man-db:amd64 2.8.5-2
2019-09-23 12:34:35 status installed plymouth-theme-ubuntu-text:amd64 0.9.4-1ubuntu1
2019-09-23 12:34:35 status installed dbus:amd64 1.12.12-1ubuntu1.1
2019-09-23 12:34:50 status installed initramfs-tools:all 0.131ubuntu19.1
```

ou autre possibilité plus optimal :

>grep "installed" /var/log/dpkg.log | tail -5

2. Utiliser dpkg et apt pour compter le nombre de paquets installés(ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage? 

Il faut se déplacer dans le dossier /var/log avec la commande :

> cd /var/log

Ensuite on vérifie le nombre de paquets présent dans le fichier dpkg.log en utilisant dpkg :

>dpkg -l | grep "^ii" | wc -l

*le grep "^ii" permet de sélectionner seulement les lignes qui commence par "ii"*

*Le résultat est 514.*

Il faut se déplacer dans le dossier /var/log/apt avec la commande :

> cd /var/log

Ensuite on vérifie le nombre de paquets présent dans le fichier dpkg.log en utilisant apt :

>apt list --installed | wc -l

*Le résultat est 515.*

3. Combien de paquets sont disponibles en téléchargement?

63 603 paquets sont disponibles en téléchargements.
On obtient cela en faisant la différence entre le nombre de paquet installés et le nombre de paquet total.

>apt list | wc -l 

(64118)

4. Créer un alias “maj” qui met à jour le système 

il faut aller dans le fichier .bashrc via la commande :
>nano .bashrc

Une fois dans le fichier .bashrc au niveau des alias il faut écrire :

``` 
alias maj="sudo apt full-upgrade"
```
On enregistre avec la commande :
>source ~/.bashrc

5. A quoi sert le paquet fortunes? Installez-le. 

Les fortunes sont de petits messages, des citations, des proverbes, etc. affichés à chaque connexion en mode console (terminal). 

Pour installer fortune il faut taper l'instruction suivante :
>sudo apt install fortune

6. Quels paquets proposent de jouer au sudoku? 

Ce logiciel est installé par défaut sur Ubuntu. Pour lancer l'application depuis le menu Applications → Jeux → Sudoku ou via la commande suivante :
> gnome-sudoku

7. Lister les derniers paquets installés explicitement avec la commande apt install

>grep "apt install" /var/log/apt/history.log

Résulat : *Commandline: apt install fortune*


## Exercice 2.

*A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2,dans la liste des commandes utiles) ? Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.*

>which -a ls | tail -1 | xargs dpkg -S

ou 

>which -a ls | xargs dpkg -S 2> /dev/null

Résultat : *coreutils: /bin/ls*

**Script origine-commande**

#!/bin/bash
echo $(which -a $1 | xargs dpkg -S 2> /dev/null)

## Exercice 3. 

*Ecrire une commande qui aﬀiche “INSTALLÉ” ou “NONINSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.*

Script : *verif_install_package* qui permet de vérifier si un paquet est installé ou non en renvoyant installé ou non installé.

```
#!/bin/bash
(dpkg -l $1 | grep "^ii") &&  echo "installé" || echo "non installé"
```
Avec le paramètre *fortunes*  :
>./verif_install_package fortunes

Résultat :

ii  fortunes       1:1.99.1-7build1 all          Data files containing fortune cookies
installé

## Exercice 4. 

*Lister les programmes livrés avec coreutils. A quoi sert la commande ’[’ et comment aﬀicher ce qu’elle retourne?*

Pour connaitre les programmes livrés avec coreutils :
>apt show coreutils

Précisement, ce paquet comprend : 

*arch base64 basename cat chcon chgrp
 chmod chown chroot cksum comm cp csplit cut date dd df dir dircolors
 dirname du echo env expand expr factor false flock fmt fold groups head
 hostid id install join link ln logname ls md5sum mkdir mkfifo mknod mktemp
 mv nice nl nohup nproc numfmt od paste pathchk pinky pr printenv printf
 ptx pwd readlink realpath rm rmdir runcon sha*sum seq shred sleep sort
 split stat stty sum sync tac tail tee test timeout touch tr true truncate
 tsort tty uname unexpand uniq unlink users vdir wc who whoami yes*

La commande '[' est un synonyme pour la commande test. C'est un manière plus lisible d'écrire le code. Il faut un crochet ouvrant et un crochet fermant.

Pour afficher ce qu'elle retourne il faut faire :
>echo $?
