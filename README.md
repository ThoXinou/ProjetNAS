# ProjetNAS

### *Ce projet a pour objectif la conception d'un NAS "maison" afin de pouvoir stocker et archiver les documents (Fichiers, Musiques, Photos, Films, etc ...) et pouvoir y accéder partout et tout le temps, dès que l'on est connecté à Internet !*

#### 1 - Matériel requis
#### 2 - Installation de Proxmox
#### 3 - La conception
#### 4 - L'utilisation


## 1 - Matériel requis
- Un "Mini-PC" -> Dans mon cas, ce sera un **Dell Optiplex 9020M D09U** avec :
  - Processeur : Intel Core i7 4785T
  - HDD : SSD Crucial MX500 de 512 Go
  - RAM : 16 Go
 
- Une clé USB bootable contenant la dernière version de Proxmox
- Un câble réseau RJ45
- Un disque dur externe (Optionnel)

## 2 - Installation de Proxmox
Tout d'abord, la 1ère étape consiste à créer une clé USB bootable contenant Proxmox afin de l'installer sur le Mini-PC.  
Proxmox est un hyperviseur de type 1 : C'est à dire qu'il va nous permettre de créer des machines virtuelles et des conteneurs, en se basant sur l'architecture du Mini-PC.

### Création de la clé USB bootable  
  
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

  
### Installation de Proxmox sur le Mini-PC

Avant de passer à l'installation, il faut modifier quelques paramètres dans le BIOS du Mini-PC
- Modifier les options de boot afin de démarrer sur la clé USB au démarrage du Mini-PC
- Activer les options de virtualisation (Intel VT-x / AMD-V), un message d'erreur apparaîtra lors du démarrage de l'installatiion de Proxmox si jamais cela n'a pas été effectué

Une fois ces réglages effectués, nous pouvons passer à l'installation de Proxmox : 
- Brancher la clé USB précédémment créée sur un des ports USB du Mini-PC
- Démarrer le Mini-PC

En toute logique, l'installation devrait débuter.

Nous arrivons sur un 1er écran, et nous allons selectionner `Install Proxmox VE (Graphical)`

![2024-08-23 13_46_20-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/a8700f66-e7fb-41ef-b025-0cb5c1412d6c)

Sur l'écran suivant, nous allons acepter les termes de l'utilisation de Proxmox en cliquant sur `Agree`

![2024-08-23 13_47_25-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/2c292534-7513-4326-ad06-e32ed42b14fd)

Sur l'écran suivant, on nous indique le disque utilisé pour l'installation de Proxmox.  
Dans le cadre de notre utlisation, nous n'avons qu'un disque dur, nous pouvons donc appuyer sur `Next`

![2024-08-23 13_47_44-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/5dec3625-57c8-4e8c-b44f-902c404e9759)

Sur l'écran suivant, nous allons definir le pays dans lequel nous sommes -> Indiquer `France`, puis cliquer sur `Next`

![2024-08-23 13_48_06-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/f9e1bca6-eb7e-430e-8006-6a1b00041461)

Sur l'écran suivant, definir un mot de passe et une adresse mail (*Qui peut être une adresse provisoire*), et cliquer sur `Next`

![2024-08-23 13_48_45-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/37d21d52-2c42-4d16-816d-0981bad81d23)

Sur l'écran suivant, il y a plusieurs informations à entrer : 
- `Hostname (FQDN)` : Indiquer le nom de notre machine ; Dans notre cas, ce sera `test.lab`
- `IP Address (CIDR)` : Indiquer une adresse IP -> En toute logique, dans un réseau domestique, les adresses IP sont situées sur le réseau `192.168.1.X`, où `X` est compris entre `2` et `254`. Dans notre cas, nous avons choisi l'adresse IP `192.168.1.10/24`
- `Gateway` : Il s'agit de la passerelle, et dans notre cas il s'agit de l'adresse IP de notre Box Internet, qui est `192.168.1.1`
- `DNS Server` : L'adresse IP de notre serveur DNS, qui est également géré par notre Box Internet, donc ce sera l'adresse `192.168.1.1`

Une fois ces informations rentrées, cliquer sur `Next`

![2024-08-23 13_49_38-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/81eb89a0-61df-4579-9699-c96dd8093588)

Et enfin, sur le dernier écran sera indiqué le récapitulatif de toutes les informations entrées précédemment, cliquer sur `Install`

![2024-08-23 13_49_57-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/2302b22f-5f19-4d08-873e-85800c341056)

La progression de l'installation sera visible via un écran de chargement : 

![2024-08-23 13_53_28-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/f5233a67-c65a-4aa1-8408-f886b1a8bf88)

> [!WARNING]
> **Une fois l'installation terminée, il vous sera demandé de retirer la clé USB, vous n'aurez que quelques secondes pour le faire alors soyez réactif !**

Au redémarrage du Mini-PC, cet écran apparaîtra : 

![2024-08-23 13_59_17-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/8486c4f0-8741-472d-ba98-c8d49f862413)

Bien que nous puissions entrer nos identifiants, **nous n'avons pas besoin de le faire !**  
  
Nous allons tout simplement entrer l'adresse indiquée, soit `https://192.168.1.10:8006/`, dans un navigateur internet sur notre PC hôte, afin de commencer la configuration de Proxmox.





