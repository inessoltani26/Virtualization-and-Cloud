# LAB02 : Configuration des Modes d’Accès Réseau sous VirtualBox

## 1. Préparation des VMs

Deux machines virtuelles ont été utilisées :

- VM1 : RHEL9-GUI
- VM2 : RHEL9-GUI Clone

VM2 a été créée à partir d’un clonage complet (Full Clone) de VM1.

<img width="488" height="129" alt="image" src="https://github.com/user-attachments/assets/d252492e-d498-439c-99ce-8913843171cb" />

*Figure 1 : Création de la machine virtuelle RHEL9-GUI Clone.*

## 2. Les modes d’accès réseau dans VirtualBox

## 3. Mode accès réseau NAT

### 3.1 Configuration du mode NAT

Le mode d’accès réseau NAT a été configuré pour les deux machines virtuelles :

- VM1 : RHEL9-GUI
- VM2 : RHEL9-GUI Clone

<img width="1014" height="597" alt="image" src="https://github.com/user-attachments/assets/07460a07-0222-4343-99b0-69f964845f3e" />

*Figure 2 : Configuration de l’adaptateur réseau en mode NAT.*

### 3.2 Vérification et tests du mode NAT

Les adresses IP des deux machines virtuelles ont été vérifiées à l’aide de la commande :

```bash
ip addr
```

| Machine | Adresse IP | Masque réseau |
|----------|----------|----------|
| VM1 : RHEL9-GUI | 10.0.2.15 | 255.255.255.0 |
| VM2 : RHEL9-GUI Clone | 10.0.2.15 | 255.255.255.0 |

*Tableau 1 : Adresse IP et masque réseau des machines.*

Les deux machines virtuelles ont obtenu l’adresse IP :

```text
10.0.2.15
```

<img width="1310" height="741" alt="image" src="https://github.com/user-attachments/assets/0def6c6c-aea3-4469-96cb-7bcc7b6ab21e" />

*Figure 3 : Vérification de l’adresse IP de VM1.*

<img width="1305" height="738" alt="image" src="https://github.com/user-attachments/assets/e8b5f1e0-8aba-447a-9dc0-dd7ab7aa43ae" />

*Figure 4 : Vérification de l’adresse IP de VM2.*

#### Tests de connectivité

La connexion Internet est fonctionnelle en mode NAT.

<img width="743" height="511" alt="image" src="https://github.com/user-attachments/assets/6dcb6aee-ce9c-454f-97a6-c9e7f3311f4e" />

*Figure 5 : Vérification de la connectivité Internet en mode NAT.*

| Source | Destination | Ping réussi ? |
|----------|----------|----------|
| Hôte | VMs | Oui |
| VMs | Hôte | Oui |
| VM1 | VM2 | Oui |
| VMs | Internet (8.8.8.8) | Oui |

*Tableau 2 : Résultats des tests de connectivité entre l’hôte, VM1, VM2 et Internet.*

## 4. Mode accès réseau par pont : Bridged Adapter

### 4.1 Configuration du mode Bridge

Le mode d’accès réseau des deux machines virtuelles a été configuré en :

```text
Bridged Adapter
```

<img width="1008" height="598" alt="image" src="https://github.com/user-attachments/assets/90e0300e-1c46-4261-856f-268ff624d2bb" />

*Figure 6 : Configuration de l’adaptateur réseau en mode Bridge de VM1.*

<img width="1012" height="605" alt="image" src="https://github.com/user-attachments/assets/2b818f89-3855-4065-bc4b-73f52b40fdcd" />

*Figure 7 : Configuration de l’adaptateur réseau en mode Bridge de VM2.*

### 4.2 Vérification et tests du mode Bridge

Les adresses IP des deux machines virtuelles ont été vérifiées à l’aide de la commande :

```bash
ip addr
```

<img width="724" height="497" alt="image" src="https://github.com/user-attachments/assets/08d42404-1c54-4770-8def-e191c9790b2d" />

*Figure 8 : Vérification de l’adresse IP de VM1.*

<img width="684" height="497" alt="image" src="https://github.com/user-attachments/assets/e8df2a41-dc1c-4985-b140-df333d70c321" />

*Figure 9 : Vérification de l’adresse IP de VM2.*

| Machine | Adresse IP | Masque réseau |
|----------|----------|----------|
| VM1 : RHEL9-GUI | 192.168.87.251 | 255.255.255.0 |
| VM2 : RHEL9-GUI Clone | 192.168.87.251 | 255.255.255.0 |

*Tableau 3 : Adresse IP et masque réseau des machines.*

#### Tests de connectivité 

La connexion Internet est fonctionnelle en mode Bridge.

| Source | Destination | Ping réussi ? |
|----------|----------|----------|
| Hôte | VMs | Oui |
| VMs | Hôte | Oui |
| VM1 | VM2 | Oui |
| VMs | Internet (8.8.8.8) | Oui |

*Tableau 4 : Résultats des tests de connectivité entre l’hôte, VM1, VM2 et Internet.*

## 5. Mode accès réseau privé : Internal Network

### 5.1 Configuration du mode Internal Network

Le mode d’accès réseau des deux machines virtuelles a été configuré en :

```text
Internal Network
```

Nom du réseau interne :

```text
intnet1
```

<img width="1015" height="604" alt="image" src="https://github.com/user-attachments/assets/a08d7c73-f8c2-4263-a850-0c10b65fb606" />

*Figure 10 : Configuration de l’adaptateur réseau de VM1 en mode Internal Network.*

<img width="1012" height="608" alt="image" src="https://github.com/user-attachments/assets/1cf18f9b-f78d-4b02-84c9-60202b39fe73" />

*Figure 11 : Configuration de l’adaptateur réseau de VM2 en mode Internal Network.*

Un serveur DHCP a été créé pour le réseau interne `intnet1` à l’aide de la commande :

```bash
VBoxManage dhcpserver add --netname intnet1 --ip 10.10.10.1 --netmask 255.255.255.0 --lowerip 10.10.10.2 --upperip 10.10.10.12 --enable
```

<img width="294" height="124" alt="image" src="https://github.com/user-attachments/assets/617783af-f05e-495f-8ad8-21ff32f7d9ef" />

*Figure 12 : Vérification de la configuration du serveur DHCP du réseau interne intnet1.*

### 5.2 Vérification et tests du mode Internal Network

Les adresses IP des deux machines virtuelles ont été vérifiées à l’aide de la commande :

```bash
ip addr
```

| Machine | Adresse IP | Masque réseau |
|----------|----------|----------|
| VM1 : RHEL9-GUI | 10.10.10.2 | 255.255.255.0 |
| VM2 : RHEL9-GUI Clone | 10.10.10.2 | 255.255.255.0 |

*Tableau 5 : Adresse IP et masque réseau des machines.*

<img width="697" height="490" alt="image" src="https://github.com/user-attachments/assets/cb6d9150-c48d-495a-86ac-9e94bc17cadf" />

*Figure 13 : Vérification de l’adresse IP de VM1 en mode Internal Network.*

<img width="694" height="500" alt="image" src="https://github.com/user-attachments/assets/5963e2f4-58be-4433-8c51-1b18b24d9da0" />

*Figure 14 : Vérification de l’adresse IP de VM2 en mode Internal Network.*

#### Tests de connectivité

Résultats :

- La communication entre VM1 et VM2 est fonctionnelle.
- Les machines virtuelles ne peuvent pas communiquer avec l’hôte.
- Les machines virtuelles ne peuvent pas accéder à Internet.

| Source | Destination | Ping réussi ? |
|----------|----------|----------|
| Hôte | VMs | Non |
| VMs | Hôte | Non |
| VM1 | VM2 | Oui |
| VMs | Internet (8.8.8.8) | Non |

*Tableau 6 : Résultats des tests de connectivité en mode Internal Network.*

## 6. Mode accès réseau privé hôte : Host-Only Adapter

### 6.1 Configuration du mode Host-Only

Un réseau Host-Only a été créé dans VirtualBox.

Configuration utilisée :

```text
Nom : vboxnet0
Adresse réseau : 192.168.56.1/24
DHCP : Enabled
```

<img width="1301" height="741" alt="image" src="https://github.com/user-attachments/assets/d2cfd6c4-e538-4dc1-a561-d042d24e82b6" />

*Figure 15 : Création du réseau Host-Only vboxnet0 dans VirtualBox.*

<img width="1009" height="608" alt="image" src="https://github.com/user-attachments/assets/bae4a397-58f9-410e-bb01-ccde5fc80aff" />

*Figure 16 : Configuration de l’adaptateur réseau de VM1 en mode Host-Only Adapter.*

<img width="1002" height="610" alt="image" src="https://github.com/user-attachments/assets/c568680d-96ea-4eb0-b3e6-b8f4aa12415d" />

*Figure 17 : Configuration de l’adaptateur réseau de VM2 en mode Host-Only Adapter.*

### 6.2 Vérification et tests du Host-Only

Les adresses IP des deux machines virtuelles ont été vérifiées à l’aide de la commande :

```bash
ip addr
```

| Machine | Adresse IP | Masque réseau |
|----------|----------|----------|
| VM1 : RHEL9-GUI | 192.168.56.101 | 255.255.255.0 |
| VM2 : RHEL9-GUI Clone | 192.168.56.101 | 255.255.255.0 |

*Tableau 7 : Adresse IP et masque réseau des machines.*

<img width="691" height="496" alt="image" src="https://github.com/user-attachments/assets/e27e1d42-86bf-4437-b790-8463849c0b4d" />

*Figure 18 : Vérification de l’adresse IP de VM1 en mode Host-Only.*

<img width="696" height="500" alt="image" src="https://github.com/user-attachments/assets/919b7f7a-9e49-42c1-adfe-2dfa1797e043" />

*Figure 19 : Vérification de l’adresse IP de VM2 en mode Host-Only.*

#### Tests de connectivité

Résultats :

- La communication entre l’hôte et les machines virtuelles est fonctionnelle.
- La communication entre VM1 et VM2 est fonctionnelle.

| Source | Destination | Ping réussi ? |
|----------|----------|----------|
| Hôte | VMs | Oui |
| VMs | Hôte | Oui |
| VM1 | VM2 | Oui |
| VMs | Internet (8.8.8.8) | Non |

*Tableau 8 : Résultats des tests de connectivité en mode Host-Only.*

## 7. Port Forwarding

### 7.1 Installation Apache

Apache a été installé sur la machine virtuelle à l’aide des commandes suivantes :

```bash
sudo dnf update -y
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

La commande suivante a été utilisée pour vérifier l’état du service :

```bash
sudo systemctl status httpd
```

Résultat :

```text
active (running)
```

<img width="699" height="511" alt="image" src="https://github.com/user-attachments/assets/cbc2bf25-8257-499b-af13-8c03c826aa91" />

*Figure 20 : Vérification du fonctionnement du service Apache.*

La page d’accueil Apache a été vérifiée à l’aide du navigateur web :

```text
http://localhost
```

<img width="1309" height="656" alt="image" src="https://github.com/user-attachments/assets/82d86124-3517-4359-96f3-f3d9d24228f9" />

*Figure 21 : Affichage de la page d’accueil du serveur Apache.*

<img width="649" height="451" alt="image" src="https://github.com/user-attachments/assets/03ad1708-76e4-4cde-8c08-841dea4429a7" />

*Figure 22 : Adresse IP de la machine virtuelle hébergeant le serveur Apache.*

### 7.2 Accès distant au serveur web

Depuis la machine hôte Ubuntu, l’accès au serveur Apache a été testé à l’aide de l’adresse IP de la machine virtuelle :

```text
http://10.0.2.15
```

Résultat :

Le serveur web n’est pas accessible depuis la machine hôte lorsque la machine virtuelle utilise le mode NAT.

<img width="1309" height="740" alt="image" src="https://github.com/user-attachments/assets/487335f3-c9fe-474f-be54-0fbd966a5ff1" />

*Figure 23 : Tentative d’accès au serveur Apache depuis la machine hôte en mode NAT.*

### 7.3 Port Forwarding : Accès distant au serveur web en mode NAT

Une règle de redirection de port a été configurée afin de permettre l’accès au serveur Apache depuis la machine hôte.

Configuration utilisée :

```text
Name : Apache
Protocol : TCP
Host Port : 3000
Guest IP : 10.0.2.15
Guest Port : 80
```

<img width="1015" height="613" alt="image" src="https://github.com/user-attachments/assets/77de8ac6-1bbb-49e7-8716-0d01db4639cf" />

*Figure 24 : Configuration de la règle de Port Forwarding.*

### 7.4 Vérification du Port Forwarding

Depuis la machine hôte, le serveur Apache a été testé à l’aide de l’adresse :

```text
http://localhost:3000
```

Résultat :

L’accès au serveur web fonctionne correctement grâce à la règle de Port Forwarding.

<img width="1296" height="738" alt="image" src="https://github.com/user-attachments/assets/09ea501b-4483-43ae-8005-9ba9b080e91c" />

*Figure 25 : Accès au serveur Apache via localhost:3000 depuis la machine hôte.*
