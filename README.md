# ProjetNAS

### *Ce projet a pour objectif la conception d'un NAS "maison" afin de pouvoir stocker et archiver les documents (Fichiers, Musiques, Photos, Films, etc ...) et pouvoir y accéder partout et tout le temps, dès que l'on est connecté à Internet !*

#### 1 - Matériel requis
#### 2 - Installation de Proxmox
#### 3 - La conception
#### 4 - L'utilisation


### 1 - Matériel requis
- Un "Mini-PC" -> Dans mon cas, ce sera un **Dell Optiplex 9020M D09U** avec :
  - Processeur : Intel Core i7 4785T
  - HDD : SSD Crucial MX500 de 512 Go
  - RAM : 16 Go
 
- Une clé USB bootable contenant la dernière version de Proxmox
- Un câble réseau RJ45
- Un disque dur externe (Optionnel)

### 2 - Installation de Proxmox
Tout d'abord, la 1ère étape consiste à créer une clé USB bootable contenant Proxmox afin de l'installer sur le Mini-PC.  
Proxmox est un hyperviseur de type 1 : C'est à dire qu'il va nous permettre de créer des machines virtuelles et des conteneurs, en se basant sur l'architecture du Mini-PC.

**Création de la clé USB bootable**  
  
Il nous faut : 
- Une clé USB (8 Go minimum)
- La dernière version de Proxmox, disponible [ici](https://enterprise.proxmox.com/iso/proxmox-ve_8.2-1.iso)
- Le logiciel Rufus, disponible [ici](https://github.com/pbatard/rufus/releases/download/v4.5/rufus-4.5.exe)
