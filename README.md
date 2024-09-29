# ProjetNAS

## *Ce projet a pour objectif la conception d'un NAS "maison" afin de pouvoir stocker et archiver les documents (Fichiers, Musiques, Photos, Films, etc ...) et pouvoir y accéder partout et tout le temps, dès que l'on est connecté à Internet !*

### 1 - Matériel requis
### 2 - Installation de Proxmox
### 3 - Configuration de Proxmox
### 4 - Installation d'un conteneur NextCloud (Stockage de fichiers)
### 5 - Installation d'une VM PfSense
### 6 - Installation de Jellyfin (Serveur Multimédia)
  
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

> [!NOTE]
> Il nous faut : 
> - Une clé USB (8 Go minimum)
> - La dernière version de Proxmox, disponible [ici](https://enterprise.proxmox.com/iso/proxmox-ve_8.2-1.iso)
> - Le logiciel Rufus, disponible [ici](https://github.com/pbatard/rufus/releases/download/v4.5/rufus-4.5.exe)

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

> [!NOTE]
> Lors de l'installation de Proxmox, seuls un écran, un clavier et une souris (Et la clé USB, cela va de soi) sont nécéssaires, vous n'avez pas besoin de relier le Mini-PC à votre Box Internet
  
Avant de passer à l'installation, il faut modifier quelques paramètres dans le BIOS du Mini-PC : 
- Modifier les options de boot afin de démarrer sur la clé USB au démarrage du Mini-PC
- Activer les options de virtualisation (Intel VT-x / AMD-V), un message d'erreur apparaîtra lors du démarrage de l'installation de Proxmox si jamais cela n'a pas été effectué
  
Une fois ces réglages effectués, nous pouvons passer à l'installation de Proxmox : 
- Brancher la clé USB précédémment créée sur un des ports USB du Mini-PC
- Démarrer le Mini-PC
  
En toute logique, l'installation devrait débuter.
  
Nous arrivons sur un 1er écran, et nous allons selectionner `Install Proxmox VE (Graphical)`

![2024-08-23 13_46_20-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/a8700f66-e7fb-41ef-b025-0cb5c1412d6c)

Sur l'écran suivant, nous allons accepter les termes de l'utilisation de Proxmox en cliquant sur `Agree`

![2024-08-23 13_47_25-Proxmox  En fonction  - Oracle VM VirtualBox](https://github.com/user-attachments/assets/2c292534-7513-4326-ad06-e32ed42b14fd)

Sur l'écran suivant, on nous indique le disque utilisé pour l'installation de Proxmox.  
  
Dans le cadre de notre utilisation, nous n'avons qu'un disque dur, nous pouvons donc appuyer sur `Next`

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

> [!TIP]
> A partir de ce moment-là, nous avons presque terminé de nous occuper du Mini-PC.
>
> Pensez bien à noter l'adresse indiquée, soit `https://192.168.1.10:8006/`
>
> Entrer les informations de connexion suivantes :
> - `test login` : `root`
> - `Password` : Le mot de passe défini précédemment
>
> Puis éxécuter la commande suivante pour éteindre le Mini-PC :
> ``` bash
> shutdown now
> ```
>  
> Le Mini-PC étant éteint, vous pouvez débrancher l'écran, le clavier, la souris, etc ... Puis placer votre Mini-PC où vous voulez, de préférence à côté de votre Box Internet et relié à celle-çi via un câble RJ45


### Bravo ! Vous pouvez dorénavant gérer votre Mini-PC directement depuis un navigateur internet
  
  
## 3 - Configuration de Proxmox

### Interface Graphique

Comme expliqué précédemment, nous n'avons plus besoin d'accéder "physiquement" au Mini-PC, celui-ci a juste besoin d'être alimenté et d'être relié au réseau domestique (Ethernet de préférence).

Afin d'accéder à l'interface graphique de Proxmox, il faut entrer l'adresse IP attribuée au Mini-PC à l'étape précédente sur un navigateur internet d'un ordinateur relié également au réseau domestique : 

![2024-09-29 12_23_18-](https://github.com/user-attachments/assets/8cebd7d9-da29-44fc-88b5-9418d3505659)

Une page avec un message indiquant que la connexion n'est pas privée apparaîtra, ce qui est normal car nous n'avons pas spécifié de certificat de sécurité.  
  
Dans notre cas, cela importe peu car les deux postes sont connectés sur notre réseau domestique, nous pouvons donc cliquer sur `Paramètres avancés`, puis sur `Continuer vers le site 192.168.1.X (dangereux)`.  

![2024-09-29 12_24_25-](https://github.com/user-attachments/assets/3e59d21f-093a-422e-8058-3ec6af86668e)

Nous arrivons donc sur la page de Proxmox, et un pop-up nous demande de nous connecter ; Pour ce faire, nous allons entrer les informations suivantes : 
 - `User name` : `root`
 - `Password` : Celui définit lors de l'installation de Proxmox
 - `Realm` : Laisser sur `Linux PAM standard authentication`
 - `Language` : A votre convenance (*La langue anglaise sera conservée pour la suite de ce tuto*)

![2024-09-29 12_25_06-](https://github.com/user-attachments/assets/9aae7786-d094-4505-85ac-d37d58f9c8e3)

Une fois validé, un pop-up apparaîtra pour nous indiquer que nous n'avons pas de souscription valide.  
  
Nous pouvons ignorer ce message et passer à la suite en apuuyant sur `OK`.  

![2024-09-29 12_25_43-toto - Proxmox Virtual Environment](https://github.com/user-attachments/assets/a6fe7403-6d7d-46e2-9c85-6df64e7a9198)

**Cette fois-çi, c'est bon, nous sommes sur l'interface principale de Proxmox !**

![2024-09-29 12_29_00-](https://github.com/user-attachments/assets/eb6a8a3e-f385-4298-9289-74d10e5177ab)


### Mises à jour

Comme tout système, il est important que Proxmox reste à jour.  
Nous allons donc cliquer sur le noeud situé sur la gauche de l'écran, puis cliquer sur `Updates`

![2024-09-29 12_29_58-](https://github.com/user-attachments/assets/3959cd66-ce0c-4dd3-b8a1-3ce6c848d18f)


Nous allons maintenant cliquer sur `Refresh`, et le même pop-up concernant la souscription apparaîtra, il suffit de l'ignorer et de cliquer sur `OK`

![2024-09-29 12_30_50-](https://github.com/user-attachments/assets/d3d80156-7bb8-48e4-b9c2-79049a93adf2)

Une fenêtre `Task Viewer` apparaîtra alors et indiquera une erreur, ce qui reste sans conséquences car, quand nous fermerons cette fenêtre, nous remarquerons que la liste des paquets à mettre à jour aura bien été téléchargée.  

![2024-09-29 12_31_20-toto - Proxmox Virtual Environment](https://github.com/user-attachments/assets/76bd3e8e-a8d7-485b-bef0-5d79e80a751a)


Il suffira donc de cliquer sur `Upgrade` afin d'effectuer les différentes mises à jour disponibles

![2024-09-29 12_29_58-](https://github.com/user-attachments/assets/83a22da2-bb01-4617-abc5-11d368427b3d)

**Bravo, Proxmox est à jour !**




























