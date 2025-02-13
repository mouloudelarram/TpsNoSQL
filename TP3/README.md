# Configuration CouchDb (simple tutorial) :
# Introduction
This guide presents a simple tutorial for using CouchDB, a document-oriented database management system. 
CouchDB is easy to install and use, and it is designed around a REST API that supports four main HTTP verbs: 
GET, PUT, POST, and DELETE.

# Setup

## Docker Setup
To run CouchDB locally using Docker, execute the following command:
```bash
docker run -d --name couchdb_demo -e COUCHDB_USER=Youssef -e COUCHDB_PASSWORD=meilleur -p 5984:5984 couchdb
```

Alternatively, you can set it up using a native installation. For more information on setting it up with Docker, refer to the official [CouchDB documentation](https://couchdb.apache.org/).

## Access CouchDB Interface
Once running, access the CouchDB interface by visiting:
```url
http://localhost:5984/_utils/
```
Use the credentials `Youssef` as the username and `meilleur` as the password.

# Operations

## REST API Basics
CouchDB supports four essential HTTP methods:

- **GET**: Retrieve a resource (e.g., get database, document).
- **PUT**: Create or replace a resource (e.g., create a new database).
- **POST**: Send data to a resource (e.g., send a document to the database).
- **DELETE**: Remove a resource (e.g., delete a document or database).

### Example: GET request
To retrieve the current configuration or information, you can use:
```bash
curl -X GET http://localhost:5984/
```

### Example: PUT request
To create a new database:
```bash
curl -X PUT http://localhost:5984/films
```

### Example: POST request
To insert a new document (assuming we are adding a document to the `films` database):
```bash
curl -X POST http://localhost:5984/films -d '{"title": "Inception", "year": 2010}'
```

### Example: DELETE request
To delete a database:
```bash
curl -X DELETE http://localhost:5984/films
```

## Advanced Usage
For more advanced operations, such as inserting collections of documents or working with documents that do not have a defined schema, please refer to the official documentation.

# Conclusion
CouchDB is a flexible and easy-to-use document store, suitable for both small and large-scale applications. It offers great support for distributed systems and is ideal for scenarios where performance and scalability are crucial. Please explore the official documentation for more details.


# Rapport sur l'implémentation de MapReduce pour le calcul de la norme de vecteurs et le produit matrice-vecteur avec CouchDB

## Introduction

Dans ce TP, nous allons explorer le concept de MapReduce dans un environnement centralisé, avec CouchDB, en appliquant ce concept sur une matrice de liens représentant des pages web. Ce type de calcul est similaire à ce que l'on trouve dans l'algorithme PageRank utilisé par des moteurs de recherche comme Google.

Le but de ce rapport est de rendre le concept de MapReduce accessible à toute personne ne connaissant pas ce concept. Pour ce faire, nous allons décomposer chaque étape du processus en termes simples, expliquer les concepts clés et détailler l'implémentation nécessaire.

---

## Partie 1 : Présentation de CouchDB

CouchDB est une base de données NoSQL orientée document, ce qui signifie qu'elle stocke des données sous forme de documents (par exemple, JSON) plutôt que de tables relationnelles. Chaque document dans CouchDB peut contenir des informations structurées ou non structurées, ce qui la rend particulièrement adaptée pour des applications web et des systèmes de gestion de données volumineuses.

Dans le cadre de ce TP, CouchDB sera utilisée pour stocker les données de notre matrice de liens entre pages web. Chaque page pourra être représentée par un document dans CouchDB, où les lignes de la matrice (représentant les pages) seront stockées sous forme de documents avec des vecteurs de poids.

---

## Partie 2 : Concept de MapReduce

MapReduce est un modèle de programmation pour traiter de grandes quantités de données de manière distribuée. Il se compose de deux étapes principales :

1. **Map** : Chaque élément de données est traité indépendamment. Dans cette étape, une fonction de mappage prend en entrée des données et génère des paires clé-valeur intermédiaires.
   
2. **Reduce** : Les paires clé-valeur produites par l'étape de mappage sont regroupées par clé, et une fonction de réduction les traite pour produire le résultat final.

MapReduce est particulièrement utile pour les calculs sur de grandes quantités de données réparties sur plusieurs machines.

---

## Partie 3 : Représentation de la matrice de liens

La matrice M est une matrice carrée de dimension N×N représentant les liens entre les pages web. Chaque élément Mij de la matrice représente le poids du lien entre la page Pi et la page Pj.

Nous allons utiliser CouchDB pour modéliser cette matrice sous forme de documents structurés. Chaque document représente une page Pi, et contiendra un vecteur de taille N représentant les liens sortants de cette page vers les autres pages. Par exemple, un document pour la page Pi pourrait ressembler à ceci :

```json
{
  "_id": "page_i",
  "links": [0.2, 0.3, 0.5, ..., 0.1]
}
```

Dans cet exemple, le tableau `links` contient les poids des liens sortants de la page Pi vers les autres pages. Chaque document représente donc une ligne de la matrice M.

---

## Partie 4 : Traitement MapReduce pour calculer la norme des vecteurs

Le calcul de la norme d'un vecteur V = (v1, v2, ..., vN) est donné par la formule :

||V|| = √(v1² + v2² + ... + vN²)

L'idée ici est de calculer la norme pour chaque vecteur (chaque ligne de la matrice) représentée dans CouchDB. Le traitement MapReduce se déroule en deux étapes :

1. **Map** : Pour chaque document de la collection C (représentant une page Pi), la fonction de map extrait le vecteur de poids `links` et émet une paire clé-valeur où la clé est l'ID de la page et la valeur est le vecteur de poids.

   Exemple de fonction Map :

   ```javascript
   function(doc) {
     emit(doc._id, doc.links);
   }
   ```

2. **Reduce** : La fonction de réduction prend les valeurs (vecteurs de poids) pour chaque clé (page) et calcule la norme de chaque vecteur. La norme est calculée comme la racine carrée de la somme des carrés des éléments du vecteur.

   Exemple de fonction Reduce :

   ```javascript
   function(keys, values, rereduce) {
     if (rereduce) {
       return values.reduce((acc, val) => acc + val, 0);
     } else {
       return Math.sqrt(values.reduce((sum, v) => sum + Math.pow(v, 2), 0));
     }
   }
   ```

---

## Partie 5 : Calcul du produit matrice-vecteur

Nous souhaitons calculer le produit de la matrice M avec un vecteur W = (w1, w2, ..., wN), où chaque élément Wj est un poids associé à la page Pj. Le résultat de ce produit est un vecteur φ où chaque élément φi est défini par :

φi = ∑ (Mij * Wj)

Le vecteur W est disponible en mémoire et accessible par toutes les fonctions Map ou Reduce. Le traitement MapReduce pour effectuer ce calcul se déroule comme suit :

1. **Map** : Pour chaque document représentant une page Pi, la fonction de mappage va associer les éléments de la ligne de la matrice Mi avec les valeurs correspondantes du vecteur W. 

   Exemple de fonction Map :

   ```javascript
   function(doc) {
     var W = [w1, w2, ..., wN];  // Vecteur W accessible ici
     var result = doc.links.map(function(link, index) {
       return link * W[index];
     });
     emit(doc._id, result);
   }
   ```

2. **Reduce** : La fonction de réduction additionne les produits obtenus pour chaque page, pour calculer le produit de la matrice avec le vecteur.

   Exemple de fonction Reduce :

   ```javascript
   function(keys, values, rereduce) {
     if (rereduce) {
       return values.reduce((acc, val) => acc + val, 0);
     } else {
       return values.reduce((acc, val) => acc + val, 0);
     }
   }
   ```

---

## Conclusion

En conclusion, ce TP vous a permis de comprendre le modèle MapReduce et comment il peut être utilisé pour effectuer des calculs complexes sur de grandes quantités de données stockées dans une base de données NoSQL comme CouchDB. Nous avons détaillé les concepts de base de MapReduce, expliqué la structure de données utilisée pour représenter la matrice de liens et montré comment implémenter des fonctions de mappage et de réduction pour effectuer les calculs demandés.

Ce modèle de calcul distribué est puissant pour traiter de grandes matrices de données, comme celles utilisées dans l'algorithme PageRank, et peut être adapté à différents types de calculs parallèles.
