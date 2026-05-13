# Guide de Déploiement et Mise à Jour sur OVH Cloud (VM)

Ce document détaille la procédure complète pour installer, configurer et mettre à jour l'application sur une machine virtuelle OVH (Ubuntu).

## Table des matières
1. [Installation Initiale](#1-installation-initiale)
2. [Mise à Jour Quotidienne](#2-mise-à-jour-quotidienne)
3. [Dépannage](#3-dépannage)

---

## 1. Installation Initiale

À effectuer une seule fois lors de la première configuration du serveur.

### 1.1 Connexion SSH à la VM
Connectez-vous à votre serveur :
```bash
ssh ubuntu@votre-ip-ovh
```

### 1.2 Configuration de la clé SSH pour GitHub
Pour cloner le dépôt privé sans mot de passe, nous utilisons une clé SSH.

1.  **Générer une clé SSH** (si elle n'existe pas déjà) :
    ```bash
    ssh-keygen -t ed25519 -C "votre_email@exemple.com"
    # Appuyez sur Entrée pour toutes les questions
    ```

2.  **Activer l'agent SSH** :
    ```bash
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519
    ```

3.  **Copier la clé publique** :
    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```
    Copiez le contenu affiché (commençant par `ssh-ed25519`).

4.  **Ajouter la clé sur GitHub** :
    *   Allez sur [GitHub Settings > SSH Keys](https://github.com/settings/keys).
    *   Cliquez sur **New SSH Key**.
    *   Collez la clé et validez.

5.  **Tester la connexion** :
    ```bash
    ssh -T git@github.com
    # Vous devriez voir : "Hi username! You've successfully authenticated..."
    ```

### 1.3 Installation de Docker (si nécessaire)
Si `docker` n'est pas installé :
```bash
# Installation rapide sur Ubuntu
sudo apt update
sudo apt install docker.io docker-compose-v2 -y
sudo usermod -aG docker $USER
# Déconnectez-vous et reconnectez-vous pour que le groupe soit pris en compte
```

### 1.4 Clonage et Lancement
1.  **Cloner le dépôt** (Utilisez le lien SSH !) :
    ```bash
    cd /home/ubuntu
    git clone git@github.com:Formasup-Mediterranee/Tinymce_editor.git
    ```

2.  **Lancer l'application** :
    ```bash
    cd Tinymce_editor
    docker compose up -d --build
    ```
    *Note : Utilisez `docker compose` (v2) et non `docker-compose` (v1).*

---

## 2. Mise à Jour Quotidienne

Pour mettre à jour l'application avec les dernières modifications du code.

1.  **Aller dans le dossier** :
    ```bash
    cd /home/ubuntu/Tinymce_editor
    ```

2.  **Récupérer le code** :
    ```bash
    git pull origin main
    ```

3.  **Redémarrer les conteneurs** :
    ```bash
    docker compose up -d --build
    ```

---

## 3. Dépannage

### Port 8080 déjà utilisé
**Erreur** : `Bind for 0.0.0.0:8080 failed: port is already allocated`
**Solution** :
1.  Trouver qui utilise le port : `sudo lsof -i :8080`
2.  Tuer le processus ou changer le port dans `docker-compose.yml` (ex: `8090:80`).

### Erreur de droits Git
**Erreur** : `fatal: detected dubious ownership in repository`
**Solution** :
```bash
git config --global --add safe.directory /home/ubuntu/Tinymce_editor
```

### Conflits de fichiers locaux
**Erreur** : `Your local changes... would be overwritten by merge`
**Solution** (Écraser les changements locaux) :
```bash
git reset --hard origin/main
git pull origin main
```

### Réinstallation Propre (Reset Total)
Si rien ne fonctionne, repartez de zéro :
```bash
cd /home/ubuntu
rm -rf Tinymce_editor
git clone git@github.com:Formasup-Mediterranee/Tinymce_editor.git
cd Tinymce_editor
docker compose up -d --build
```
