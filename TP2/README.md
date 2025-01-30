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
db.films.count()
```

### 2. Structure des Documents
Pour comprendre la structure d'un document :
```javascript
db.films.findOne()
```

### 3. Films d'Action
Pour afficher la liste des films d'action :
```javascript
db.films.find({"genre": "Action"})
```

### 4. Nombre de Films d'Action
Pour compter le nombre de films d'action :
```javascript
db.films.count({"genre": "Action"})
```

### 5. Films d'Action Français
Pour trouver les films d'action produits en France :
```javascript
db.films.find({"genre": "Action", "pays": "France"})
```

### 6. Films d'Action Français de 1963
```javascript
db.films.find({"genre": "Action", "pays": "France", "annee": 1963})
```

### 7. Filtrage des Attributs
Pour n'afficher que certains attributs des films d'action français :
```javascript
db.films.find(
    {"genre": "Action", "pays": "France"},
    {"titre": 1, "annee": 1}
)
```

### 8. Masquer les Identifiants
Pour masquer les identifiants des documents :
```javascript
db.films.find(
    {"genre": "Action", "pays": "France"},
    {"_id": 0, "titre": 1, "annee": 1}
)
```

### 9. Titres et Grades sans ID
```javascript
db.films.find(
    {"genre": "Action", "pays": "France"},
    {"_id": 0, "titre": 1, "grades": 1}
)
```

### 10. Filtrage par Note
Pour les films avec une note supérieure à 10 :
```javascript
db.films.find(
    {"genre": "Action", "pays": "France", "grades": {$gt: 10}},
    {"_id": 0, "titre": 1, "grades": 1}
)
```

### 11. Films avec Uniquement des Notes Supérieures à 10
```javascript
db.films.find(
    {"genre": "Action", "pays": "France", "grades": {$not: {$lte: 10}}},
    {"_id": 0, "titre": 1, "grades": 1}
)
```

### 12. Genres Uniques
Pour afficher tous les genres distincts :
```javascript
db.films.distinct("genre")
```

### 13. Grades Uniques
```javascript
db.films.distinct("grades")
```

### 14. Recherche par Artistes
Pour trouver les films avec certains artistes :
```javascript
db.films.find({
    "artistes": {
        $in: ["artist:4", "artist:18", "artist:11"]
    }
})
```

### 15. Films sans Résumé
```javascript
db.films.find({"resume": {$exists: false}})
```

### 16. Films avec Leonardo DiCaprio en 1997
```javascript
db.films.find({
    "artistes": "Leonardo DiCaprio",
    "annee": 1997
})
```

### 17. Films avec Leonardo DiCaprio ou de 1997
```javascript
db.films.find({
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
