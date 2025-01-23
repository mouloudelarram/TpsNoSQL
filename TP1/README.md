# Introduction à Redis et Commandes de Base

Ce README fournit une vue d'ensemble de l'utilisation de Redis, une base de données clé-valeur souvent utilisée pour des opérations à haute performance. Il explique les commandes de base de Redis, le travail avec les clés, la gestion de la persistance et l'utilisation de structures de données comme les listes et les ensembles. Redis est bien documenté, et ce guide sert de référence introductive.

---

## Table des Matières
1. [Qu'est-ce que Redis?](#qu'est-ce-que-redis)
2. [Prérequis](#prérequis)
3. [Installation](#installation)
4. [Fonctionnalités clés de Redis](#Fonctionnalités-clés-de-Redis)
5. [Commandes de Base](#commandes-de-base)
6. [Travailler avec les Listes](#travailler-avec-les-listes)
7. [Travailler avec les Ensembles](#travailler-avec-les-ensembles)
8. [Persistance des Données](#persistance-des-données)
9. [Apprentissage Supplémentaire](#apprentissage-supplémentaire)
10. [Introduction aux Ensembles Ordonnés](#introduction-aux-ensembles-ordonnés)
11. [Commandes de Base pour les Ensembles Ordonnés](#commandes-de-base-pour-les-ensembles-ordonnés)
12. [Importance de la Persistance et des Performances](#importance-de-la-persistance-et-des-performances)
13. [Introduction aux Haches (Hashes)](#introduction-aux-haches-hashes)
14. [Command de Base pour les Haches](#commandes-de-base-pour-les-haches)
15. [Utilisation des Pub/Sub pour les Applications en Temps Réel](#utilisation-des-pubsub-pour-les-applications-en-temps-réel)
16. [Command de Base pour Pub/Sub](#commandes-de-base-pour-pubsub)
17. [Gestion des Bases de Données](#gestion-des-bases-de-données)
18. [Commandes de Base pour la Gestion des Bases de Données](#commandes-de-base-pour-la-gestion-des-bases-de-données)
19. [Conclusion](#conclusion)


---

## Qu'est-ce que Redis?
   Redis est un système de gestion de bases de données NoSQL orienté clé-valeur, connu pour sa rapidité et sa flexibilité. Avant de plonger dans l'installation et l'utilisation de Redis, il est important de comprendre les quatre principales familles de systèmes de gestion de bases de données NoSQL :
1. Orientées graphes : Utilisées pour traiter les réseaux massifs, comme les recommandations sur Twitter2.
2. Orientées colonnes : Facilitent les traitements privilégiant les colonnes, par exemple pour calculer l'âge moyen des utilisateurs2.
3. Orientées clé-valeur : Offrent une efficacité en lecture/écriture et un bon changement d'échelle, comme pour les systèmes de sauvegarde type Dropbox2.
4. Orientées document : Supportent des structures variables, idéales pour la gestion des métadonnées de produits sur des plateformes comme eBay2.

Redis appartient à la famille des bases de données orientées clé-valeur. Voici comment l'installer et l'utiliser :

---

## Prérequis
- Connaissances de base des bases de données et de la ligne de commande.
- Redis installé sur votre machine.

---

## Installation
### Installer Redis sur Linux :
1. Mettre à jour les listes de paquets :
   ```bash
   sudo apt update
   ```
2. Installer Redis :
   ```bash
   sudo apt install redis-server
   ```
3. Démarrer le service Redis :
   ```bash
   sudo systemctl start redis
   ```

### Installer Redis sur macOS (avec Homebrew) :
1. Installer Redis :
   ```bash
   brew install redis
   ```
2. Démarrer Redis :
   ```bash
   redis-server
   ```

### Vérifier l'Installation :
Utiliser la commande :
```bash
redis-cli ping
```
Cela devrait retourner :
```
PONG
```

---
## Fonctionnalités clés de Redis
#### Redis offre plusieurs fonctionnalités importantes :
1. Modèle d'exécution mono-thread : Redis utilise principalement un modèle mono-thread pour traiter les requêtes, ce qui offre simplicité, prévisibilité et performance3.
2. Pipeline de commandes : Redis permet d'envoyer plusieurs commandes en une seule requête, réduisant ainsi la latence réseau3.
3. Transactions : Redis supporte les transactions, permettant l'exécution atomique d'un groupe de commandes3.
4. Pub/Sub (Publication/Abonnement) : Redis intègre un système de messagerie permettant aux clients de s'abonner à des canaux et de recevoir des messages publiés3.
5. Réplication : Redis prend en charge la réplication maître-esclave, améliorant la disponibilité des données et permettant de répartir la charge de lecture3.
6. Cluster Redis : Introduit dans Redis 3.0, le cluster permet une meilleure gestion des défaillances et une haute disponibilité7.
7. Compression de chaînes : Redis utilise une technique de compression pour réduire l'utilisation de la mémoire pour les petites chaînes de caractères3.
8. Structures de données avancées : En plus des simples paires clé-valeur, Redis supporte des structures de données plus complexes comme les listes, les ensembles, et les hashes.

Redis se distingue par sa rapidité, sa flexibilité et sa capacité à fonctionner comme une base de données en mémoire avec persistance optionnelle. Ces caractéristiques en font un choix populaire pour les caches, les files d'attente de messages, et les applications nécessitant des performances élevées en lecture/écriture.

---

## Commandes de Base

### Démarrer le Serveur
Pour démarrer Redis, exécuter :
```bash
redis-server
```
Pour interagir avec lui, utiliser le CLI Redis :
```bash
redis-cli
```

### Définir et Gérer les Clés
1. **Définir une paire clé-valeur** :
   ```bash
   SET mykey "hello"
   ```
2. **Obtenir une valeur** :
   ```bash
   GET mykey
   ```
3. **Supprimer une clé** :
   ```bash
   DEL mykey
   ```
4. **Incrémenter une clé** :
   ```bash
   INCR counter
   ```
5. **Décrémenter une clé** :
   ```bash
   DECR counter
   ```
6. **Spécifier une durée de vie d'une clé :** :
   ```bash
   expire macle 120
   ```
7. **Afficher la durée de vie d'une clé :** :
   ```bash
   ttl macle
   ```
---

## Travailler avec les Listes
Les listes Redis permettent de stocker des collections ordonnées de chaînes de caractères.

1. **Ajouter des éléments à une liste** :
   - Pousser à gauche :
     ```bash
     LPUSH mylist "item1"
     ```
   - Pousser à droite :
     ```bash
     RPUSH mylist "item2"
     ```
2. **Récupérer des éléments d'une liste** :
   ```bash
   LRANGE mylist 0 -1
   ```
   (Cela récupère tous les éléments.)
2Bis. **Récupérer des éléments d'une liste** :
   ```bash
   LRANGE mylist 0 0
   ```
   (Cela récupère le 1er élément.)
4. **Extraire des éléments d'une liste** :
   - Extraire à gauche :
     ```bash
     LPOP mylist
     ```
   - Extraire à droite :
     ```bash
     RPOP mylist
     ```

---

## Travailler avec les Ensembles
Les ensembles Redis stockent des éléments uniques.

1. **Ajouter des éléments à un ensemble** :
   ```bash
   SADD myset "item1" "item2" "item3"
   ```
2. **Récupérer tous les éléments d'un ensemble** :
   ```bash
   SMEMBERS myset
   ```
3. **Supprimer un élément d'un ensemble** :
   ```bash
   SREM myset "item1"
   ```
4. **Effectuer des opérations d'union entre ensembles** :
   ```bash
   SUNION set1 set2
   ```

---

## Persistance des Données
Redis stocke les données en mémoire mais peut les persister sur disque. Pour configurer la persistance :
1. Ouvrir le fichier de configuration Redis :
   ```bash
   sudo nano /etc/redis/redis.conf
   ```
2. Modifier la directive `save` pour définir quand les données doivent être sauvegardées (par exemple, après un nombre spécifique de changements ou un intervalle de temps).

### Exemple :
```conf
save 900 1
save 300 10
save 60 10000
```
Cela sauvegarde le jeu de données si :
- 1 changement se produit en 15 minutes.
- 10 changements se produisent en 5 minutes.
- 10,000 changements se produisent en 1 minute.

Redémarrer Redis après avoir effectué des modifications :
```bash
sudo systemctl restart redis
```

---

## Apprentissage Supplémentaire
Redis offre une documentation extensive sur son site officiel. Vous pouvez explorer les commandes, les configurations et les cas d'utilisation avancés ici :
[Documentation Redis](https://redis.io/docs)

### Exemples de Recherches :
- Pour en savoir plus sur les listes :
  ```bash
  HELP LIST
  ```
- Pour explorer les commandes pour les ensembles :
  ```bash
  HELP SET
  ```

---

## Introduction aux Ensembles Ordonnés

Les ensembles ordonnés (zsets) sont une structure de données utilisée pour classer des scores, par exemple pour des systèmes de recommandation. Contrairement aux ensembles simples (sets), les ensembles ordonnés permettent de stocker des valeurs avec un score associé.

## Commandes de Base pour les Ensembles Ordonnés

1. **Définir des valeurs avec un score** :
   ```bash
   ZADD score "Augustin" 19
   ZADD score "Inès" 18
   ```

2. **Récupérer les éléments** :
   ```bash
   ZRANGE score 0 -1
   ```
   Cette commande renvoie tous les éléments par ordre croissant de score.

3. **Récupérer les éléments dans un intervalle spécifique** :
   ```bash
   ZRANGE score 0 1
   ```
   Cette commande renvoie les deux premiers éléments par ordre croissant.

4. **Récupérer les éléments par ordre décroissant** :
   ```bash
   ZREVRANGE score 0 -1
   ```

5. **Connaître la position d'un élément** :
   ```bash
   ZRANK score "Augustin"
   ```

## Importance de la Persistance et des Performances

Il est crucial de comprendre que les opérations de lecture/écriture sur disque sont beaucoup plus lentes que celles en mémoire RAM. Redis, en tant que base de données en mémoire, minimise ces accès disque, ce qui améliore considérablement les performances.

## Introduction aux Haches (Hashes)

Les haches permettent de stocker des paires clé-valeur où chaque clé peut avoir plusieurs champs. C'est utile pour distribuer des données sur plusieurs serveurs.

## Commandes de Base pour les Haches

1. **Définir une hache** :
   ```bash
   HSET user:1 username "Mouloud"
   HSET user:1 age 22
   HSET user:1 email "mouloud@mail.fr"
   ```

2. **Récupérer toutes les valeurs d'une hache** :
   ```bash
   HGETALL user:1
   ```

3. **Définir plusieurs champs en une seule commande** :
   ```bash
   HMSET user:2 username "Augustin" age 5 email "augustin@gmail.fr"
   ```

4. **Incrémenter une valeur dans une hache** :
   ```bash
   HINCRBY user:2 age 4
   ```

## Utilisation des Pub/Sub pour les Applications en Temps Réel

Les pub/sub (publication/souscription) sont utilisés pour les applications en temps réel, comme les notifications et les messages.

## Commandes de Base pour Pub/Sub

1. **Souscrire à un canal** :
   ```bash
   SUBSCRIBE mychannel
   ```

2. **Publier un message sur un canal** :
   ```bash
   PUBLISH mychannel "Nouveau cours sur MongoDB"
   ```

3. **Souscrire à plusieurs canaux** :
   ```bash
   PSUBSCRIBE my*
   ```

## Gestion des Bases de Données

Redis permet de gérer plusieurs bases de données. Par défaut, il y a 16 bases de données disponibles.

## Commandes de Base pour la Gestion des Bases de Données

1. **Changer de base de données** :
   ```bash
   SELECT 1
   ```

2. **Lister toutes les clés d'une base de données** :
   ```bash
   KEYS *
   ```

## Conclusion

Redis est un outil puissant pour les applications nécessitant des performances élevées. Il offre une grande flexibilité avec ses différentes structures de données et ses commandes de base. Pour plus d'informations, consultez la [documentation officielle de Redis](https://redis.io/docs).
