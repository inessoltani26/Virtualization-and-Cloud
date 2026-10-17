# LAB01 : Installation et Gestion de Machines Virtuelles sous VirtualBox

## 1. Prérequis

- **Machine hôte :** Ubuntu 22.04
- **Hyperviseur :** Oracle VirtualBox
- **Système invité :** Red Hat Enterprise Linux 9.8
- **Image ISO :** rhel-9.8-x86_64-dvd.iso

## 2. Création et configuration d’une machine virtuelle avec VirtualBox

### 2.1 Création de la machine virtuelle
- Nom : RHEL9-GUI
- Type : Linux
- Distribution : Red Hat
- Version : Red Hat (64-bit)
- RAM : 4096 MB
- CPU : 2
- Disque : 40 Go (VDI)
  
<img width="549" height="740" alt="image" src="https://github.com/user-attachments/assets/595a3a42-72bf-4f00-90b3-9d4ac5960320" />

*Figure 1 : Configuration de la machine virtuelle RHEL9-GUI.*

### 2.2 Installation du système invité

**Configuration choisie :**
- Langue : English
- Clavier : French
- Fuseau horaire : Africa/Tunis
- Type d’installation : Server with GUI
- Utilisateur : ines

<img width="1373" height="864" alt="image" src="https://github.com/user-attachments/assets/d36f689f-74cb-4613-ae6d-481e9d939551" />

*Figure 2 : Vérification de l'installation de Red Hat Enterprise Linux 9.8.*

## 3. Installation des suppléments invité : "Guest Additions"

### 3.1 Installation des modules noyau nécessaires

**Commandes exécutées :**

```bash
sudo dnf update -y
sudo dnf install -y gcc gcc-c++ make kernel-devel kernel-headers
```

<img width="1339" height="709" alt="image" src="https://github.com/user-attachments/assets/40725d83-3ed7-4975-b4cc-6d4c15464b1c" />

*Figure 3 : Installation des modules noyau nécessaires à la compilation des Guest Additions.*

### 3.2 Méthode 1 : Installation des Guest Additions via l'ISO

Le média Guest Additions a été inséré depuis VirtualBox :

```text
Devices → Insert Guest Additions CD Image...
```

L'installation a été lancée à l'aide de la commande :

```bash
sudo sh /run/media/ines/VBox_GAs_7.2.20/VBoxLinuxAdditions.run
```

### 3.3 Méthode 2 : Installation des Guest Additions depuis les dépôts

Cette méthode n'a pas été réalisée.

L'installation des Guest Additions a été effectuée via l'image ISO (Méthode 1).

### 3.4 Vérification de l'installation

Commande utilisée :

```bash
lsmod | grep vbox
```

<img width="1384" height="867" alt="image" src="https://github.com/user-attachments/assets/4e4b8922-28a2-45a0-b683-68c809dd692a" />

*Figure 4 : Vérification du chargement des modules VirtualBox Guest Additions.*

## 4. Partage du Presse-papiers entre hôte et invité

Configuration appliquée :

- Shared Clipboard : Bidirectional
- Drag and Drop : Bidirectional

Des tests de copier-coller ont été réalisés avec succès dans les deux sens :

- Ubuntu → RHEL
- RHEL → Ubuntu

<img width="999" height="610" alt="image" src="https://github.com/user-attachments/assets/1e65458d-99cc-4938-a463-7904e20c573f" />

*Figure 5 : Configuration du partage du presse-papiers et du glisser-déposer.*

## 5. Partage des dossiers entre hôte et invité

Un dossier partagé nommé `Shared_Folder` a été configuré entre Ubuntu et la machine virtuelle RHEL 9.8.

### 5.1 Étape 1 : Configuration hyperviseur

Dans VirtualBox, un dossier partagé a été ajouté avec les paramètres suivants :

- Folder Name : Shared_Folder
- Auto-Mount : Yes

<img width="1010" height="606" alt="image" src="https://github.com/user-attachments/assets/746d8f30-60ec-4fa8-bda9-eeae5dc6f6db" />

*Figure 6 : Configuration du dossier partagé Shared_Folder dans VirtualBox.*

### 5.2 Étape 2 : Commande "mount"

La commande suivante a été utilisée pour vérifier le montage du dossier partagé :

```bash
mount | grep vboxsf
```

Le dossier partagé a été monté avec succès dans le système invité.

<img width="809" height="531" alt="image" src="https://github.com/user-attachments/assets/4c4b2d40-746e-4da4-9d82-24ea6b241ebb" />

*Figure 7 : Vérification du montage du dossier partagé dans la machine virtuelle.*

### 5.3 Configuration permanente

Une entrée a été ajoutée dans le fichier /etc/fstab afin de rendre le montage du dossier partagé permanent.

Commande utilisée :

```bash
sudo nano /etc/fstab
```

Ligne ajoutée :

```text
Shared_Folder /mnt vboxsf defaults 0 0
```

Après redémarrage de la machine virtuelle, le dossier partagé reste accessible automatiquement.

Commande de vérification :

```bash
mount | grep vboxsf
```

Résultat obtenu :

```text
Shared_Folder on /mnt type vboxsf
```

<img width="708" height="512" alt="image" src="https://github.com/user-attachments/assets/01ce09f0-77a8-48f6-8088-99a510e1633b" />

*Figure 8 : Vérification du montage automatique du dossier partagé après redémarrage de la machine virtuelle.*

## 6. Export/Import d’une machine virtuelle

### 6.1 Exportation d’une VM

La machine virtuelle `RHEL9-GUI` a été exportée au format OVA à l’aide de VirtualBox.

Étapes réalisées :

1. Arrêt de la machine virtuelle.
2. Sélection de :

```text
File → Export Appliance
```

3. Sélection de la machine virtuelle `RHEL9-GUI`.
4. Choix du format OVA.
5. Lancement de l’exportation.

<img width="1095" height="477" alt="image" src="https://github.com/user-attachments/assets/826f72b7-1c55-43ab-bb72-03c8589aaa55" />

*Figure 9 : Sélection du fichier OVA généré lors de l’exportation de la machine virtuelle.*

### 6.2 Importation d’une VM

Le fichier OVA généré a été importé dans VirtualBox.

Étapes réalisées :

1. Sélection de :

```text
File → Import Appliance
```

2. Sélection du fichier OVA précédemment exporté.
3. Vérification des paramètres proposés.
4. Lancement de l’importation.

La machine virtuelle importée a été créée avec succès sous le nom :

```text
RHEL9-GUI 1
```

<img width="502" height="131" alt="image" src="https://github.com/user-attachments/assets/abb20709-6d1b-498e-adcd-6883cb0f6140" />

*Figure 10 : Machine virtuelle importée avec succès sous le nom RHEL9-GUI 1.*

## 7. Clonage d’une machine virtuelle

Un clonage complet (Full Clone) a été réalisé à partir de la machine virtuelle RHEL9-GUI.

### Étapes réalisées

1. Sélection de la machine virtuelle à cloner.
2. Clic droit sur la machine virtuelle.
3. Sélection de l’option **Clone**.
4. Attribution du nom :

```text
RHEL9-GUI Clone
```

5. Sélection du type :

```text
Full Clone
```

6. Validation de l’opération.

### Résultat obtenu

Une nouvelle machine virtuelle indépendante a été créée sous le nom :

```text
RHEL9-GUI Clone
```

<img width="495" height="202" alt="image" src="https://github.com/user-attachments/assets/70d30114-44bf-4aaf-ba1e-a8e456b94207" />

*Figure 11 : Création d’un clone complet de la machine virtuelle RHEL9-GUI sous le nom RHEL9-GUI Clone.*

## 8. Gestion des snapshots

Création et restauration d’un snapshot.

### 8.1 Création d’un Snapshot

Un snapshot a été créé pour la machine virtuelle `RHEL9-GUI`.

Snapshot créé :

```text
Before_Modification
```

Étapes réalisées :

1. Sélection de la machine virtuelle.
2. Accès à l’onglet **Snapshots**.
3. Sélection de **Take Snapshot**.
4. Attribution du nom `Before_Modification`.
5. Validation de l’opération.

<img width="1299" height="671" alt="image" src="https://github.com/user-attachments/assets/d34cdc16-5f81-4346-adb8-7ab5d655b2a1" />

*Figure 12 : Snapshot Before_Modification créé dans VirtualBox.*

### 8.2 Restauration d’un Snapshot

Le snapshot `Before_Modification` a été restauré afin de revenir à un état précédent de la machine virtuelle.

Étapes réalisées :

1. Accès à l’onglet **Snapshots**.
2. Sélection du snapshot `Before_Modification`.
3. Sélection de l’option **Restore**.
4. Validation de la restauration.

Après restauration, les modifications effectuées après la création du snapshot ont été supprimées.

<img width="685" height="489" alt="image" src="https://github.com/user-attachments/assets/8b9f0a44-95f5-4666-bf01-9f31e8be4e3c" />

*Figure 13 : Vérification du retour à l’état enregistré dans le snapshot Before_Modification.*
