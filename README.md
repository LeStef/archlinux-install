# archlinux-install
Résumé des commandes nécéssaires à une installation d'archlinux

## Formatage du disque
1. Partition de Boot
-"n" pour new partition
-"p" pour primary
-partition number 'default'
-first sector 'default'
-"+200M" pour la taille

2. Partition swap
même chose que pour la partition de boot, sauf que la taille doit etre égale à au moins une fois et demie la taille de la RAM

3. Partition root
Même chose que pour la partition de boot, hormis +25G en taille

4. Partition home
Même chose que pour la partition de boot, hormis que la taille prend le reste du disque

"w" pour écrire la table de partition sur le disque

## Définition des filesystems

`mkfs.ext4 /dev/sdb1 (boot)`

`mkfs.ext4/dev/sdb3 (root)`

`mkfs.ext4 /dev/sdb4 (home)`
