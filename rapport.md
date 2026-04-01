# LAB 1 : Environnement linux embarqué :


## Work
Create a new partition:
```bash
Command (m for help): n
Partition type
   p   primary partition (1-4)
   e   extended
p
Partition number (1-4): 3
First sector (16-31176703, default 16): 4327504
Last sector or +size{,K,M,G,T} (4327504-31176703, default 31176703):
Using default value 31176703

Command (m for help): p
Disk /dev/mmcblk2: 15 GB, 15962472448 bytes, 31176704 sectors
487136 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk2p1 *  0,33,50     8,74,18           2128     133199     131072 64.0M  c Win95 FAT32 (LBA)
/dev/mmcblk2p2    8,74,19     269,95,34       133200    4327503    4194304 2048M 83 Linux
/dev/mmcblk2p3    1023,3,16   1023,3,16      4327504   31176703   26849200 12.8G 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table
fdisk: WARNING: rereading partition table failed, kernel still uses old table: Device or resource busy

```

demarrage app à l'init : 

```bash
#!/bin/sh

case "$1" in
  start)
    echo "Démarrage de mon programme..."
    /opt/monprog &
    ;;
  stop)
    echo "Arrêt de mon programme..."
    pkill -f /opt/monprog
    ;;
  restart)
    $0 stop
    $0 start
    ;;
  *)
    echo "Usage: $0 {start|stop|restart}"
    exit 1
    ;;
esac

exit 0
```

## Questions :

### 1. Comment faut-il procéder pour générer l’U-Boot ?

U-Boot est géré directement par Buildroot :
```bash
cd /buildroot
make
```
Génération de l'image finale avec U-boot inclus.

### 2. Comment peut-on ajouter et générer un package supplémentaire dans Buildroot ?

**Package existant dans Buildroot :**
```bash
make menuconfig # Ajouter le package voulu dans le config
make            
```

### 3. Comment doit-on procéder pour modifier la configuration du noyau Linux ?

```bash
make linux-menuconfig # Modifier les configs
make            
```

### 4. Comment faut-il faire pour générer son propre rootfs ?

On peut générer son rootfs en utilisant buildroot avec soit le `menuconfig` ou soit le rootfs overlay.

### 5. Comment faudrait-il procéder pour utiliser la carte eMMC en lieu et place de la carte SD ?

Depuis la target, il faut flasher le contenu de la carte SD sur la eMMC.
Puis, il faut modifier les bootargs en changant le script de boot, pour le faire boot depuis l'eMMC.

### 6. Quelle serait la configuration optimale pour le développement uniquement d’applications en espace utilisateur ?

La configuration **CIFS/SMB (montage réseau du rootfs)** est la plus adaptée.
Cela va permettre d'installer les applications utilisateurs très facilement, sans à avoir à reflasher la SD à chaque fois.

Pas vraiment d'interet à mettre le kernel en TFTP dans ce cas là, vu qu'on faire principalement que du développement utilisateur.