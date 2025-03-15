# CheckpointBanc3
Configuration réseau et accès SSH (GNU/Linux SRVLX01)
Configurez l’interface réseau en mode Bridge sur VirtualBox.

Assurez-vous que DHCP est activé pour IPv4 et que l'auto-configuration IPv6 fonctionne.
![image](https://github.com/user-attachments/assets/b04878e0-c6ca-440f-ac8f-51b1d9bb306f)

![image](https://github.com/user-attachments/assets/7df4a76c-19d7-4065-abd0-71188f7afd81)
Vous pouvez vérifier cela dans /etc/network/interfaces ou en utilisant netplan (selon la distribution).

Installez et activez OpenSSH :

Installez le package avec sudo apt install openssh-server.

Assurez-vous que le service SSH est actif avec sudo systemctl start ssh et qu’il démarre automatiquement (sudo systemctl enable ssh).

sudo apt update && sudo apt install -y openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh


![image](https://github.com/user-attachments/assets/1e00f2d0-6dec-4ac3-84f8-47b76f555de4)


## Exercice 1

### Partie 1 : Gestion des utilisateurs
Configuration des utilisateurs (Windows Server)

Q.1.1.1 : Créer l'utilisateur Lionel Lemarchand avec les mêmes attributs que Kelly Rhameur
Connectez-vous à votre serveur Windows SRVWIN01.

Ouvrez l'outil Utilisateurs et ordinateur Active Directory (ADUC)

Localisez l'utilisateur Kelly Rhameur :

Cliquez droit sur son compte, puis choisissez Copier.

Remplissez les informations pour Lionel Lemarchand :

Nom d’utilisateur, prénom, mot de passe, etc.

Validez en cliquant sur Créer.
![image](https://github.com/user-attachments/assets/f3440880-44cc-45e1-b7f0-27318f457e21)


Q.1.1.2 : Créer une OU DeactivatedUsers et déplacer le compte désactivé de Kelly Rhameur dedans
Dans UOAD, faites un clic droit sur le domaine ou une autre OU existante.

Choisissez Nouvelle > Unité d'organisation (OU).

Nommez cette OU "DeactivatedUsers" et validez.

Désactivez le compte de Kelly Rhameur :
Clic droit sur son compte, puis Désactiver le compte.
![image](https://github.com/user-attachments/assets/2f69eddd-b76b-454f-893e-5b2ec446fe18)

Déplacez le compte vers "DeactivatedUsers" :

Faites un clic droit, sélectionnez Déplacer, et choisissez la nouvelle OU.
![image](https://github.com/user-attachments/assets/7b9e1c27-31c9-4ad0-8c03-eb741a2a42d2)


Q.1.1.3 : Modifier le groupe de l'OU dans laquelle était Kelly Rhameur
Toujours dans ADUC, localisez l'OU d'origine de Kelly Rhameur.

Sélectionnez les Propriétés de l'OU.

Accédez à l'onglet Groupes, puis ajoutez ou supprimez les groupes nécessaires.

Q.1.1.4 : Archiver le dossier de Kelly Rhameur et créer celui de Lionel Lemarchand
Accédez à l'emplacement des profils utilisateurs sur le serveur.
Compressez (archivez) le dossier de Kelly Rhameur, par exemple avec un outil comme ZIP.

Déplacez l’archive dans un emplacement sécurisé ou dédié pour l’archivage.

Créez un nouveau dossier pour Lionel Lemarchand, avec les mêmes permissions que Kelly Rhameur.

### Partie 2 : Restriction utilisateurs
Q.1.2.1 Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.

![image](https://github.com/user-attachments/assets/1dd3286d-3d0d-47f2-9331-5fb5b2e8b443)

Q.1.2.2 Bloquer sa connexion au seul ordinateur CLIENT01.

![image](https://github.com/user-attachments/assets/34875723-6a93-4571-8f69-6aa3f790a89b)

Q.1.2.3 Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.
Ouvrir Group Policy Management (GPMC) sur SRVWIN01.

Créer une nouvelle GPO nommée PasswordPolicy_LabUsers.

Modifier la GPO et configurer les paramètres suivants dans :
Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy :

![image](https://github.com/user-attachments/assets/bffb95b0-e258-4e2e-8dc4-5a4c438e6656)

Lier la GPO à l'OU .

Forcer l'application de la stratégie avec :
gpupdate /force

Q.1.3.1 - Créer une GPO Drive-Mount pour monter les lecteurs E: et F: sur les clients

Ouvrir Group Policy Management (GPMC) sur SRVWIN01.

Créer une nouvelle GPO nommée Drive-Mount.

Modifier la GPO et naviguer vers :
User Configuration → Preferences → Windows Settings → Drive Maps.

Ajouter deux nouvelles entrées :

Lecteur E:

Action : Créer

Location : \\Serveur\PartageE

Reconnect : Cocher

Drive Letter : E:

Lecteur F:

Action : Créer

Location : \\Serveur\PartageF

Reconnect : Cocher

Drive Letter : F:

Lier la GPO à l'OU concernée (ex: LabUsers).

Forcer l'application de la stratégie avec :

![image](https://github.com/user-attachments/assets/2469babe-4791-42e0-9a7f-6f81be5329c8)

## Exercice 2 : Manipulations pratiques sur VM Linux

### Partie 1 : Gestion des utilisateurs

Q.2.1.1 - Création d'un compte utilisateur personnel

Se connecter au serveur SRVLX01.

Exécuter la commande suivante pour créer un utilisateur :

sudo adduser fatouma

Suivre les instructions pour définir un mot de passe et renseigner les informations supplémentaires si nécessaire.

![image](https://github.com/user-attachments/assets/aa7faf95-28a3-4b45-a0e7-4b02ab428877)

Q.2.1.2 - Préconisations pour le compte utilisateur

Utiliser un mot de passe fort :

Minimum 12 caractères, incluant majuscules, minuscules, chiffres et caractères spéciaux.

Ne pas utiliser le compte root directement :

Ajouter l'utilisateur au groupe sudo pour les privilèges administratifs :

sudo usermod -aG sudo fatouma

Désactiver la connexion SSH directe pour root :

Modifier le fichier de configuration SSH :

sudo nano /etc/ssh/sshd_config

Changer la ligne PermitRootLogin yes en PermitRootLogin no.
![image](https://github.com/user-attachments/assets/6dfaad04-92cb-43f5-98f6-fdd192b8cc94)


Redémarrer le service SSH :

sudo systemctl restart ssh

Configurer l'expiration du mot de passe :

Définir une expiration à 60 jours :

sudo chage -M 60 fatouma

Activer l'authentification à deux facteurs (2FA) pour SSH (optionnel mais recommandé).


### Partie 2 : Configuration de SSH

Q.2.2.1 - Désactiver complètement l'accès à distance de l'utilisateur root

Modifier le fichier de configuration SSH :

sudo nano /etc/ssh/sshd_config

Trouver la ligne suivante et la modifier :

PermitRootLogin no

Redémarrer le service SSH :

sudo systemctl restart ssh

Q.2.2.2 - Autoriser l'accès à distance à ton compte personnel uniquement

Modifier le fichier de configuration SSH :

sudo nano /etc/ssh/sshd_config

Ajouter ou modifier la ligne suivante :

AllowUsers mon_utilisateur

Redémarrer le service SSH :

sudo systemctl restart ssh

Q.2.2.3 - Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe

Générer une clé SSH sur la machine cliente :

ssh-keygen -t rsa -b 4096

Copier la clé publique sur le serveur :

ssh-copy-id mon_utilisateur@adresse_du_serveur
![image](https://github.com/user-attachments/assets/ddcc1125-0344-4e33-9008-dd8c1a3424b3)

Modifier le fichier de configuration SSH :

sudo nano /etc/ssh/sshd_config

Mettre à jour les paramètres suivants :

PasswordAuthentication no
PubkeyAuthentication yes

Redémarrer le service SSH :
