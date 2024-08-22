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

Dans un premier temps, nous allons devoir créer la clé USB bootable.  
Pour ce faire, nous allons utiliser le logiciel Rufus cité précédemment.  
Une fois Rufus téléchargé et lancé, nous arrivons sur cette fenêtre : 

![2024-08-22 11_53_59-Rufus 4 5 2180](https://github.com/user-attachments/assets/8b58a9ea-22f5-46bd-84e4-fcb001dca097)

Cliquer sur `SELECTION` et choisir le fichier ISO correspondant à Proxmox, il se nomme `proxmox-ve_<version>.iso`  
Une fois cela fait, un pop-up apparaîtra, cliquer sur `OK`

![2024-08-22 11_55_45-Rufus 4 5 2180](https://github.com/user-attachments/assets/359f5c8d-b1f3-4a74-bde5-a010b5e9e895)

Une fois cela fait, cliquer sur `DEMARRER`  
Un pop-up apparaîtra, indiquant que toutes les données contenues sur la clé USB seront perdues  
Cliquer sur `OK`

![2024-08-22 11_56_36-Rufus 4 5 2180](https://github.com/user-attachments/assets/a2a7e601-10de-4db8-9f7b-dd27e6109fcc)

La création de la clé USB bootable va commencer

![2024-08-22 11_57_13-Rufus 4 5 2180](https://github.com/user-attachments/assets/974e41a1-686d-4135-9fdc-421152dc4718)

Une fois terminé, vous pourrez fermer le logiciel et retirer la clé USB pour commencer l'installation de Proxmox

![2024-08-22 11_59_44-Rufus 4 5 2180](https://github.com/user-attachments/assets/2c1ecef5-0d50-49e4-8344-e13360474e56)




