# LAB03 — Introduction à Docker (version Red Hat Enterprise Linux 9)

## Prérequis

Les prérequis du TP ont été vérifiés à l’aide des commandes suivantes :

```bash
uname -r
sudo subscription-manager status
sudo dnf repolist
```

<img width="704" height="503" alt="image" src="https://github.com/user-attachments/assets/93010dbd-b14e-4b91-964f-9c4135660e46" />

*Figure 1 : Vérification des prérequis nécessaires à l’installation de Docker.*

## 1. Installation de Docker sous RHEL 9

### 1.1 Méthode 1 : Docker CE depuis le dépôt officiel Docker

#### Étape 1 : Retirer les paquets en conflit

Les paquets susceptibles d’entrer en conflit avec Docker CE ont été supprimés.

```bash
sudo dnf remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-selinux \
docker-engine-selinux \
docker-engine
```

<img width="861" height="528" alt="image" src="https://github.com/user-attachments/assets/0db4a24a-db70-444a-94c5-83c6963550c8" />

*Figure 2 : Suppression des paquets en conflit avec Docker CE.*

#### Étape 2 : Installation des outils nécessaires et ajout du dépôt Docker

Le paquet suivant a été vérifié :

```bash
sudo dnf install -y dnf-plugins-core
```

Résultat :

```text
dnf-plugins-core is already installed.
```

Le dépôt officiel Docker a ensuite été ajouté :

```bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```

<img width="842" height="489" alt="image" src="https://github.com/user-attachments/assets/0d16b505-638e-4c0d-b92f-af36205f98f6" />

*Figure 3 : Ajout du dépôt officiel Docker sous RHEL 9.*

###### Installation de Docker CE

Docker CE a été installé à l’aide de la commande :

```bash
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Les composants suivants ont été installés :

- docker-ce
- docker-ce-cli
- containerd.io
- docker-buildx-plugin
- docker-compose-plugin

<img width="848" height="504" alt="image" src="https://github.com/user-attachments/assets/72b3e5e6-718b-468c-82df-4aa798bee80e" />

*Figure 4 : Installation de Docker CE et de ses composants.*

###### Démarrage du service Docker

Le service Docker a été activé et démarré à l’aide des commandes suivantes :

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Vérification du service :

```bash
sudo systemctl status docker
```

Résultat :

```text
active (running)
```

<img width="869" height="492" alt="image" src="https://github.com/user-attachments/assets/c1f9df3e-1f89-4ce4-a2e7-157fbc21282a" />

*Figure 5 : Vérification du fonctionnement du service Docker.*

### 1.2 Méthode 2 : Podman depuis les dépôts Red Hat

Cette méthode n'a pas été finalisée.

Une tentative d'installation a été effectuée à l'aide de la commande :

```bash
sudo dnf install -y podman-docker
```

L'installation n'a pas pu être réalisée en raison d'un conflit avec Docker CE déjà installé sur le système.

<img width="877" height="506" alt="image" src="https://github.com/user-attachments/assets/c2f082e1-cb89-44b0-8dee-bdef5a7353ed" />

*Figure 6 : Conflit entre Docker CE et Podman lors de l'installation de podman-docker.*

### 1.3 Post-installation (méthode 1 uniquement)

#### 1.3.1 Gestion du service Docker

Le service Docker a été activé et démarré à l’aide des commandes suivantes :

```bash
sudo systemctl enable --now docker
sudo systemctl status docker
```

Résultat :

```text
Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled)
Active: active (running)
```

<img width="884" height="510" alt="image" src="https://github.com/user-attachments/assets/49919b3c-e1f6-4e50-aa52-d74ff9d1940d" />

*Figure 7 : Vérification du service Docker après activation.*

### 1.3.2 Accord des privilèges nécessaires

Le groupe Docker a été vérifié puis l’utilisateur a été ajouté à ce groupe.

Commandes exécutées :

```bash
getent group docker
sudo usermod -aG docker $USER
```

<img width="869" height="501" alt="image" src="https://github.com/user-attachments/assets/d1949f68-d348-4ce5-bf3b-9bf57836d22f" />

*Figure 8 : Vérification de l’existence du groupe docker et ajout de l’utilisateur au groupe.*

Après reconnexion de l’utilisateur, la présence du groupe a été vérifiée :

```bash
id -nG
```

Résultat :

```text
ines wheel docker
```

Vérification de la version de Docker :

```bash
docker --version
```

Résultat :

```text
Docker version 29.9.1
```

Vérification des informations Docker :

```bash
docker info | grep -E "Server Version|Storage Driver|Cgroup Version"
```

Résultat :

```text
Server Version: 29.9.1
Storage Driver: overlayfs
Cgroup Version: 2
```

<img width="679" height="483" alt="image" src="https://github.com/user-attachments/assets/1207cff9-b23e-489a-aaf5-293d8652036b" />

*Figure 9 : Vérification du groupe docker et des informations de la plateforme Docker.*

## 2. Utilisation de la commande Docker

Les commandes Docker suivantes ont été utilisées pour afficher les informations disponibles :

```bash
docker
```

```bash
docker run --help
```

### 2.1 Gestion des images

#### 2.1.1 Lister les images

La liste des images Docker a été affichée à l’aide de la commande :

```bash
docker images
```

Aucune image Docker n’était présente sur le système au moment de l’exécution de la commande.

<img width="696" height="492" alt="image" src="https://github.com/user-attachments/assets/0514fb49-aee9-459b-825b-326a86bdf66f" />

*Figure 10 : Liste des images Docker disponibles sur le système.*

#### 2.1.2 Recherche des images

Les commandes suivantes ont été utilisées pour rechercher des images Docker :

```bash
docker search centos
```

```bash
docker search --filter=stars=5 centos
```

<img width="1004" height="579" alt="image" src="https://github.com/user-attachments/assets/ec4e4d8a-07ce-4458-828a-ed6732a4d25c" />

*Figure 11 : Recherche des images Docker relatives à CentOS.*

<img width="1065" height="587" alt="image" src="https://github.com/user-attachments/assets/b42a96c3-2822-4a62-943d-7b026c781fdc" />

*Figure 12 : Recherche des images Docker avec le filtre stars=5.*

#### 2.1.3 Télécharger une image

Commande exécutée :

```bash
docker pull centos
```

##### 1 : Quel est le message d'erreur obtenu ?

```text
Error response from daemon: failed to resolve reference "docker.io/library/centos:latest": docker.io/library/centos:latest: not found
```

##### 2 : Pourquoi cette erreur se produit ?

L’image `centos:latest` n’est plus disponible sur Docker Hub.

##### 3 : Quelle version peut être utilisée à la place ?

```bash
docker pull quay.io/centos/centos:centos7
```

<img width="871" height="352" alt="image" src="https://github.com/user-attachments/assets/1dd81408-bffd-4565-bee0-50fcbbbf7e44" />

*Figure 13 : Erreur obtenue lors du téléchargement de l’image centos:latest.*

<img width="862" height="530" alt="image" src="https://github.com/user-attachments/assets/2d891c9a-f021-4c54-a0c3-480c5549bd7c" />

*Figure 14 : Téléchargement réussi de l’image quay.io/centos/centos:centos7.*

#### Bonus RHEL : Téléchargement d’une image UBI

Une image UBI (Universal Base Image) a été téléchargée depuis le registre Red Hat à l’aide de la commande :

```bash
docker pull registry.access.redhat.com/ubi9/ubi
```

La liste des images disponibles a ensuite été vérifiée :

```bash
docker images
```

Deux images sont désormais disponibles :

- quay.io/centos/centos:centos7
- registry.access.redhat.com/ubi9/ubi:latest

<img width="892" height="543" alt="image" src="https://github.com/user-attachments/assets/7470784f-3cb2-48d9-924b-bdf53e1aa762" />

*Figure 15 : Téléchargement de l’image UBI et vérification des images Docker disponibles.*

#### 2.1.4 Supprimer une image

La suppression d’une image Docker a été réalisée à l’aide de la commande :

```bash
docker rmi quay.io/centos/centos:centos7
```

Résultat :

```text
Deleted: sha256:e4ca2ed0202e76be184e75fb26d14bf974193579039d5573fb2348664deef76e
```

La liste des images a ensuite été vérifiée :

```bash
docker images
```

Une seconde suppression a été réalisée à l’aide de la commande :

```bash
docker rmi $(docker images -a -q)
```

Résultat :

Toutes les images Docker ont été supprimées.

<img width="875" height="531" alt="image" src="https://github.com/user-attachments/assets/b72b29d7-0c33-494c-89c2-cf9170717038" />

*Figure 16 : Suppression des images Docker et vérification de la liste des images.*

## 2.2 Gestion des conteneurs

### 2.2.1 Créer un conteneur

La création d’un conteneur a été réalisée à l’aide de la commande :

```bash
docker create --name MyFirstContainer quay.io/centos/centos:centos7
```

Résultat :

Le conteneur `MyFirstContainer` a été créé avec succès.

<img width="878" height="333" alt="image" src="https://github.com/user-attachments/assets/4c2be551-887c-44d9-b5cd-e7355620a5b5" />

*Figure 17 : Création du conteneur MyFirstContainer.*

#### 2.2.2 Lister les conteneurs actifs

La liste des conteneurs actifs a été affichée à l’aide de la commande :

```bash
docker ps
```

Résultat :

Aucun conteneur actif n’était présent au moment de l’exécution de la commande.

<img width="798" height="246" alt="image" src="https://github.com/user-attachments/assets/b54cdc9a-2af7-4976-b45a-d229a253693e" />

*Figure 18 : Liste des conteneurs actifs.*

#### 2.2.3 Lister tous les conteneurs

La liste de tous les conteneurs a été affichée à l’aide de la commande :

```bash
docker ps -a
```

Résultat :

Le conteneur `MyFirstContainer` apparaît dans la liste avec l’état :

```text
Created
```

<img width="777" height="338" alt="image" src="https://github.com/user-attachments/assets/347254ab-50c8-46eb-b718-4582eb8b8035" />

*Figure 19 : Affichage de tous les conteneurs Docker.*

#### 2.2.4 Démarrer un conteneur

Le conteneur a été démarré à l’aide de la commande :

```bash
docker start MyFirstContainer
```

Résultat :

```text
MyFirstContainer
```

<img width="781" height="225" alt="image" src="https://github.com/user-attachments/assets/4d4401ba-b6ab-4994-853d-07757ff309b2" />

*Figure 20 : Démarrage du conteneur MyFirstContainer.*

#### 2.2.5 Arrêter un conteneur

Le conteneur a été arrêté à l’aide de la commande :

```bash
docker stop MyFirstContainer
```

Résultat :

```text
MyFirstContainer
```

<img width="769" height="210" alt="image" src="https://github.com/user-attachments/assets/0493d69c-1a5f-47c3-ba17-b0da9f759f06" />

*Figure 21 : Arrêt du conteneur MyFirstContainer.*

#### 2.2.6 Créer et démarrer un conteneur

La création et le démarrage d’un conteneur ont été réalisés à l’aide de la commande :

```bash
docker run quay.io/centos/centos:centos7
```

Résultat :

Le conteneur a été créé puis exécuté avec succès.

<img width="772" height="241" alt="image" src="https://github.com/user-attachments/assets/e25e923b-1eb4-4124-8f10-b24d86b3d848" />

*Figure 22 : Création et exécution d’un conteneur Docker à partir de l’image CentOS 7.*

#### 2.2.7 Supprimer un conteneur

Le conteneur a été supprimé à l’aide de la commande :

```bash
docker rm MyFirstContainer
```

Résultat :

```text
MyFirstContainer
```

Le conteneur a été supprimé avec succès.

<img width="799" height="241" alt="image" src="https://github.com/user-attachments/assets/05090135-eb04-4376-af09-cff9f941902d" />

*Figure 23 : Suppression du conteneur MyFirstContainer.*

#### 2.2.8 Exécuter une commande dans le conteneur

Deux conteneurs ont été exécutés à l’aide des commandes suivantes :

```bash
docker run --rm ubuntu uname -a
```

```bash
docker run --rm centos:centos7 uname -a
```

La commande suivante a également été exécutée sur la machine hôte :

```bash
uname -a
```

Résultat :

Les commandes exécutées dans les conteneurs Ubuntu et CentOS affichent le même noyau Linux que celui de la machine hôte.

<img width="756" height="509" alt="image" src="https://github.com/user-attachments/assets/9d6a380c-430f-4db6-ab35-df07308a6d16" />

*Figure 24 : Exécution de la commande uname -a dans les conteneurs Ubuntu et CentOS ainsi que sur la machine hôte.*

## 3. Image Docker hello-world

La commande suivante a été exécutée :

```bash
docker run hello-world
```

Résultat :

L’image `hello-world` a été téléchargée automatiquement puis exécutée avec succès.

Le message :

```text
Hello from Docker!
```

confirme que l’installation Docker fonctionne correctement.

<img width="768" height="589" alt="image" src="https://github.com/user-attachments/assets/2b489ad4-56ef-40d3-9821-99691de6989c" />

*Figure 25 : Exécution de l’image hello-world.*

