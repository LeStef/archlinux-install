# archlinux-install
Résumé des commandes nécéssaires à une installation d'archlinux

## Formatage du disque
1. Partition de Boot

* "n" pour new partition
* "p" pour primary
* partition number 'default'
* first sector 'default'
* "+200M" pour la taille

2. Partition swap

Même chose que pour la partition de boot, sauf que la taille doit etre égale à au moins une fois et demie la taille de la RAM

3. Partition root

Même chose que pour la partition de boot, hormis +25G en taille

4. Partition home

Même chose que pour la partition de boot, hormis que la taille prend le reste du disque

"w" pour écrire la table de partition sur le disque

## Définition des filesystems

1. Partition boot

`mkfs.ext4 /dev/sdb1`

2. Partition root

`mkfs.ext4/dev/sdb3`

3. Partition home

`mkfs.ext4 /dev/sdb4`

4. Partition swap

`mkswap /dev/sda2`

`swapon /dev/sda2`

## Montage des partitions

On monte la partition root :

`mount /dev/sda3 /mnt`

On se place dans le point de montage :

`cd /mnt`

On créé les dossiers home et boot :

`mkdir /mnt/home`

`mkdir /mnt/boot`

On monte les partitions dans ces emplacements :

`mount /dev/sdb1 /mnt/boot`

`mount /dev/sdb4 /mnt/home`

On vérifie que tout est ok

`lsblk`

## Installation des paquets de base

On passe la commande magique pacstrap :

`pacstrap /mnt base base-devel vim`

## Générer le fichier fstab

Pour générer le fstab avec les UUID des partitions, -U est utile

`genfstab -U /mnt >> /mnt/etc/fstab`

---
DANS LE SYSTEM
---

On chroot pour installer le bootloader sur le disque

`arch-root /mnt`


## Selection mirroir

`vim /etc/pacman.d/mirrorlist`

Ne sélectionner que quelques proches mirroirs, mettre en commentaire les autres


## Installation d’un networkmanager

`pacman -S networkmanager`

Mise en marche au prochain reboot

`systemctl enable NetworkManager`

## Installation de Grub

`pacman -S grub`

Installation de grub sur le dd

`grub-install --target=i386-pc /dev/sdb`

Génération du fichier de configuration associé

`grub-mkconfig -o /boot/grub/grub.cfg`


## Définition d’un password pour root

`passwd`


## Génération des locales

`vim /etc/locale.gen`

Décommenter les lignes nécéssaires, puis

`locale-gen`

puis :

`echo LANG="fr_FR.UTF-8" > /etc/locale.conf`

## Disposition du clavier

`echo KEYMAP=fr > /etc/vconsole.conf`

## Heure système

`ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime`

## Hostname

`vim /etc/hostname`

`archlinux-pc`

Puis : 

`echo '127.0.1.1 archlinux-pc.localdomain archlinux-pc' >> /etc/hosts`

## Sortie et reboot

Sortir du chroot :

`exit`

Umount des partitions :

`umount -R /mnt`

Reboot :

`reboot`

## Environnement graphique

1. Installation de Xorg

`pacman -Syu xorg-server xorg-xinit`

2. Installation d'un environnement de test Xorg

Pour avoir un environnement minimal de test, en attendant d'avoir installé et configuré votre environnement graphique, vous pouvez installer les paquets suivants (vous permettant ainsi de lancer le gestionnaire de fenêtres Twm par un simple startx sans disposer de .xinitrc dans votre $HOME, par l'intermédiaire du /etc/X11/xinit/xinitrc ): 

`pacman -S xorg-twm xorg-xclock xterm`

Pour un test rapide et sans risque de X (il se fermera tout seul au bout de 10s), vous pouvez créer un fichier .xinitrc de test et lancer startx : 

```
echo "xterm & sleep 10" > ~/.xinitrc
startx
```

3. Clavier

`vim /etc/X11/xorg.conf.d/10-keyboard-layout.conf`

```
Section "InputClass"
    Identifier         "Keyboard Layout"
    MatchIsKeyboard    "yes"
    Option             "XkbLayout"  "fr"
    Option             "XkbVariant" "latin9" # accès aux caractères spéciaux plus logique avec "Alt Gr" (ex : « » avec "Alt Gr" w x)
EndSection
```

4. Polices

Voici une sélection de paquets à installer pour avoir des belles polices bien lissées dans toutes les applis graphiques et ne plus jamais se poser de question à ce sujet. 

`pacman -S xorg-fonts-type1 ttf-dejavu artwiz-fonts font-bh-ttf font-bitstream-speedo gsfonts sdl_ttf ttf-bitstream-vera        ttf-cheapskate ttf-liberation ttf-freefont ttf-arphic-uming ttf-baekmuk`

5. Xfce

`pacman -S xfce4 xfce4-goodies`

Pour le lancer manuellement : 

`startxfce4`

Sinon :

`pacman -S lightdm`

LigthDM nécessite l'installation d'un greeter (interface utilisateur), le greeter par défaut est lightdm-gtk-greeter et n'est pas installé de base par le paquet lightdm. Pour l'installer : 

`pacman -S lightdm-gtk-greeter`

Pour l'activer : 

`systemctl enable lightdm.service`

Pour le démarrer sans rebooter : 

`systemctl start lightdm.service`


