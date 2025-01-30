
# MongoDB Installation et Exécution

Ce guide explique comment installer et exécuter MongoDB sur **Windows**, **macOS**, et **Linux**.

## Table des matières

- [Prérequis](#prérequis)
- [Installation sur Windows](#installation-sur-windows)
- [Installation sur macOS](#installation-sur-macos)
- [Installation sur Linux](#installation-sur-linux)
- [Exécution de MongoDB](#exécution-de-mongodb)
- [Vérification de l'installation](#vérification-de-linstallation)
- [Dépannage](#dépannage)

---

## Prérequis

Avant d'installer MongoDB, assurez-vous que vous avez les éléments suivants :

- **Windows** : Un système d'exploitation Windows 7 ou plus récent.
- **macOS** : Une version macOS 10.11 ou supérieure.
- **Linux** : Une distribution basée sur Debian ou Red Hat (ex. Ubuntu, Fedora).

---

## Installation sur Windows

### 1. Télécharger MongoDB

1. Rendez-vous sur la page de téléchargement officielle de MongoDB : [Télécharger MongoDB](https://www.mongodb.com/try/download/community).
2. Sélectionnez **Windows** comme système d'exploitation.
3. Choisissez **MSI** comme type d'installateur.

### 2. Installer MongoDB

1. Lancez le fichier `.msi` téléchargé et suivez les étapes de l'assistant d'installation.
2. **Options recommandées** : Laissez les options par défaut, sauf si vous avez une préférence spécifique.
3. **Installation en tant que service** : Assurez-vous que l'option "Install MongoDB as a Service" est activée.

### 3. Ajouter MongoDB au PATH

1. Après l'installation, ouvrez l'Explorateur de fichiers et accédez au dossier d'installation de MongoDB (par défaut `C:\Program Files\MongoDB\Server\X.X\bin`).
2. Ajoutez ce chemin au **PATH** de votre système :
   - Cliquez avec le bouton droit sur **Ce PC** > **Propriétés** > **Paramètres système avancés**.
   - Cliquez sur **Variables d'environnement** > **Path** > **Modifier**.
   - Ajoutez le chemin de MongoDB à la liste et cliquez sur **OK**.

---

## Installation sur macOS

### 1. Installer MongoDB avec Homebrew

Si vous n'avez pas encore installé **Homebrew** (gestionnaire de paquets pour macOS), vous pouvez l'installer avec la commande suivante dans votre terminal :

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. Installer MongoDB

1. Ouvrez un terminal et exécutez la commande suivante pour installer MongoDB :

```bash
brew tap mongodb/brew
brew install mongodb-community@6.0
```

### 3. Lancer MongoDB

Une fois l'installation terminée, vous pouvez démarrer MongoDB avec la commande :

```bash
brew services start mongodb/brew/mongodb-community
```

---

## Installation sur Linux (ex. Ubuntu)

### 1. Ajouter le dépôt MongoDB

Sur **Ubuntu**, vous pouvez installer MongoDB en utilisant les étapes suivantes.

1. **Importer la clé publique de MongoDB** :

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
```

2. **Ajouter le dépôt MongoDB** :

```bash
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

3. **Mettre à jour la liste des paquets** :

```bash
sudo apt-get update
```

### 2. Installer MongoDB

Installez MongoDB avec cette commande :

```bash
sudo apt-get install -y mongodb-org
```

### 3. Démarrer MongoDB

Lancez MongoDB avec la commande suivante :

```bash
sudo systemctl start mongod
```

Si vous voulez que MongoDB démarre automatiquement à chaque démarrage du système :

```bash
sudo systemctl enable mongod
```

---

## Exécution de MongoDB

### Démarrer MongoDB

- **Windows** : Si vous avez installé MongoDB en tant que service, il démarre automatiquement. Sinon, ouvrez un terminal PowerShell et tapez :

  ```bash
  mongod
  ```

- **macOS et Linux** : Utilisez la commande suivante pour démarrer MongoDB manuellement :

  ```bash
  mongod
  ```

### Utiliser le client MongoDB (`mongosh`)

Ouvrez un nouveau terminal et tapez :

```bash
mongosh
```

Cela ouvrira le client MongoDB interactif.

---

## Vérification de l'installation

1. **Vérifier si le serveur MongoDB fonctionne** :
   - Ouvrez un terminal et tapez :

   ```bash
   ps aux | grep mongod
   ```

   Si vous voyez `mongod` dans les résultats, cela signifie que le serveur MongoDB fonctionne.

2. **Vérifier la connexion avec `mongosh`** :
   - Tapez `mongosh` dans le terminal pour ouvrir le client MongoDB et assurez-vous de pouvoir interagir avec la base de données.

---

## Dépannage

### MongoDB ne démarre pas

- **Problème de répertoire de données manquant** :
  Si vous obtenez une erreur du type "Data directory not found", vous devez créer le répertoire de données :

  - **Windows** : Créez le répertoire `C:\data\db`.
  - **macOS/Linux** : Créez le répertoire `/data/db`.

  ```bash
  sudo mkdir -p /data/db
  ```

- **Problème de permission** :
  Si vous avez une erreur de permission, vérifiez les droits d'accès sur le répertoire de données ou lancez MongoDB avec des droits d'administrateur.

### Commande `mongo` ou `mongosh` non reconnue

Si vous obtenez une erreur disant que la commande `mongo` ou `mongosh` est introuvable, assurez-vous que le répertoire `bin` de MongoDB est ajouté à la variable d'environnement `PATH`.

---

## Conclusion

Vous avez maintenant MongoDB installé et fonctionnant sur votre machine Windows, macOS ou Linux. Vous pouvez commencer à interagir avec votre base de données en utilisant `mongosh` pour exécuter des commandes et importer des données. Si vous rencontrez des problèmes, consultez les sections de dépannage ou la documentation officielle de MongoDB.

---




# 📌 Installation des outils MongoDB (MongoDB Database Tools)

Ce guide explique comment installer les outils MongoDB sur un système Debian-based (ex. : Kali Linux, Ubuntu, Debian).

## ✅ Étape 1 : Ajouter la clé GPG de MongoDB
MongoDB utilise une clé GPG pour sécuriser son dépôt. Exécutez la commande suivante pour l'ajouter à votre système :

```sh
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo tee /usr/share/keyrings/mongodb-server-key.asc
```

---

## ✅ Étape 2 : Ajouter le dépôt officiel de MongoDB
Ajoutez le dépôt MongoDB à votre liste des sources en exécutant la commande suivante :

```sh
echo "deb [signed-by=/usr/share/keyrings/mongodb-server-key.asc] https://repo.mongodb.org/apt/debian bullseye/mongodb-org/6.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

Assurez-vous de remplacer **bullseye** par votre version de Debian si nécessaire (ex. : `buster` pour Debian 10).

---

## ✅ Étape 3 : Mettre à jour et installer les outils MongoDB
Mettez à jour la liste des paquets et installez MongoDB Database Tools avec :

```sh
sudo apt update
sudo apt install mongodb-database-tools
```

---

## ✅ Étape 4 : Vérifier l’installation
Une fois l'installation terminée, vérifiez si les outils sont correctement installés en exécutant :

```sh
mongoimport --version
```


# TP MongoDB - Gestion d'une Base de Films

Ce TP permet de se familiariser avec MongoDB en manipulant une collection de films. Il couvre les opérations de base de requêtage et de filtrage des données.

## Prérequis

- MongoDB installé sur votre machine
- Le fichier `films.json` contenant les données à importer

## Installation et Import des Données

1. Importez la collection de films avec la commande :
```bash
./mongoimport --db lesfilms --collection films films.json --jsonArray
```

## Requêtes MongoDB

### 1. Vérification de l'Import
Pour vérifier que les données ont été importées correctement :
```javascript
db.Film.count()
```

### 2. Structure des Documents
Pour comprendre la structure d'un document :
```javascript
db.Film.findOne()
```

### 3. Films d'Action
Pour afficher la liste des films d'action :
```javascript
db.Film.find({"genre": "Action"})
```

### 4. Nombre de Films d'Action
Pour compter le nombre de films d'action :
```javascript
db.Film.count({"genre": "Action"})
```

### 5. Films d'Action Français
Pour trouver les films d'action produits en France :
```javascript
db.Film.find({"genre": "Action", "pays": "France"})
```

### 6. Films d'Action Français de 1963
```javascript
db.Film.find({"genre": "Action", "pays": "France", "annee": 1963})
```

### 7. Filtrage des Attributs
Pour n'afficher que certains attributs des films d'action français :
```javascript
db.Film.find(
    {"genre": "Action", "pays": "France"},
    {"titre": 1, "annee": 1}
)
```

### 8. Masquer les Identifiants
Pour masquer les identifiants des documents :
```javascript
db.Film.find(
    {"genre": "Action", "pays": "France"},
    {"_id": 0, "titre": 1, "annee": 1}
)
```

### 9. Titres et Grades sans ID
```javascript
db.Film.find(
    {"genre": "Action", "pays": "France"},
    {"_id": 0, "titre": 1, "grades": 1}
)
```

### 10. Filtrage par Note
Pour les films avec une note supérieure à 10 :
```javascript
db.Film.find(
    {"genre": "Action", "pays": "France", "grades": {$gt: 10}},
    {"_id": 0, "titre": 1, "grades": 1}
)
```

### 11. Films avec Uniquement des Notes Supérieures à 10
```javascript
db.Film.find(
    {"genre": "Action", "pays": "France", "grades": {$not: {$lte: 10}}},
    {"_id": 0, "titre": 1, "grades": 1}
)
```

### 12. Genres Uniques
Pour afficher tous les genres distincts :
```javascript
db.Film.distinct("genre")
```

### 13. Grades Uniques
```javascript
db.Film.distinct("grades")
```

### 14. Recherche par Artistes
Pour trouver les films avec certains artistes :
```javascript
db.Film.find({
    "artistes": {
        $in: ["artist:4", "artist:18", "artist:11"]
    }
})
```

### 15. Films sans Résumé
```javascript
db.Film.find({"resume": {$exists: false}})
```

### 16. Films avec Leonardo DiCaprio en 1997
```javascript
db.Film.find({
    "artistes": "Leonardo DiCaprio",
    "annee": 1997
})
```

### 17. Films avec Leonardo DiCaprio ou de 1997
```javascript
db.Film.find({
    $or: [
        {"artistes": "Leonardo DiCaprio"},
        {"annee": 1997}
    ]
})
```

## Notes Importantes

- Les requêtes peuvent être exécutées dans la console MongoDB ou via un client MongoDB
- Assurez-vous d'être connecté à la bonne base de données (`lesfilms`)
- Les résultats peuvent varier selon le contenu de votre fichier `films.json`
- Certaines requêtes peuvent nécessiter des index pour de meilleures performances

## Dépannage

Si l'import échoue, vérifiez que :
- Le fichier films.json est au bon format
- MongoDB est en cours d'exécution
- Vous avez les droits suffisants pour créer la base de données
