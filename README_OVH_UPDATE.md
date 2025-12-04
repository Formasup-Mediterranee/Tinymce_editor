# Mise à jour sur OVH Cloud (VM)

Ce guide explique comment mettre à jour l'application sur votre machine virtuelle OVH.

## Prérequis

*   Accès SSH à la VM.
*   L'application doit déjà être déployée et configurée avec Docker et Docker Compose.

## Procédure de mise à jour

1.  **Connexion SSH**
    Connectez-vous à votre VM via SSH :
    ```bash
    ssh user@votre-ip-ovh
    ```

2.  **Accéder au dossier du projet**
    Naviguez vers le répertoire où l'application est installée (le chemin peut varier selon votre installation) :
    ```bash
    cd /chemin/vers/Tinymce_editor
    ```

3.  **Récupérer la dernière version du code**
    Téléchargez les dernières modifications depuis le dépôt Git :
    ```bash
    git pull origin main
    ```

4.  **Redémarrer les conteneurs**
    Reconstruisez et redémarrez les conteneurs pour prendre en compte les changements (configuration Nginx, nouveaux fichiers, etc.) :
    ```bash
    docker-compose up -d --build
    ```

## Vérification

Une fois la commande terminée, vous pouvez vérifier que les conteneurs tournent correctement :

```bash
docker-compose ps
```

Accédez ensuite à votre application via votre navigateur pour confirmer que la mise à jour a bien été prise en compte.
