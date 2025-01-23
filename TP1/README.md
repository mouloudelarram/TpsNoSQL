# Introduction à Redis et Commandes de Base

Ce README fournit une vue d'ensemble de l'utilisation de Redis, une base de données clé-valeur souvent utilisée pour des opérations à haute performance. Il explique les commandes de base de Redis, le travail avec les clés, la gestion de la persistance et l'utilisation de structures de données comme les listes et les ensembles. Redis est bien documenté, et ce guide sert de référence introductive.

---

## Table des Matières
1. [Qu'est-ce que Redis?](#qu'est-ce-que-redis)
2. [Prérequis](#prérequis)
3. [Installation](#installation)
4. [Commandes de Base](#commandes-de-base)
5. [Travailler avec les Listes](#travailler-avec-les-listes)
6. [Travailler avec les Ensembles](#travailler-avec-les-ensembles)
7. [Persistance des Données](#persistance-des-données)
8. [Apprentissage Supplémentaire](#apprentissage-supplémentaire)

---

## Qu'est-ce que Redis?
Redis est une base de données clé-valeur en mémoire qui supporte une grande variété de structures de données telles que les chaînes de caractères, les listes, les ensembles, les hachages, et plus encore. Elle est couramment utilisée pour le cache, l'analyse en temps réel, et d'autres applications nécessitant un stockage rapide et éphémère. Redis peut également persister les données sur disque pour un stockage à plus long terme.

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
3. **Extraire des éléments d'une liste** :
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

Redis est un outil puissant qui peut améliorer les performances de vos applications. Ce guide ne couvre que les bases—explorez ses capacités complètes sur le [site web de Redis](https://redis.io/).
