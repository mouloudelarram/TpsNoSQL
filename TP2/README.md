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
