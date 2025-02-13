Le but de ce rapport est de rendre le concept de MapReduce accessible à toute personne ne connaissant pas ce concept. Pour ce faire, nous allons décomposer chaque étape du processus en termes simples, expliquer les concepts clés et détailler l'implémentation nécessaire.

Partie 1 : Présentation de CouchDB
CouchDB est une base de données NoSQL orientée document, ce qui signifie qu'elle stocke des données sous forme de documents (par exemple, JSON) plutôt que de tables relationnelles. Chaque document dans CouchDB peut contenir des informations structurées ou non structurées, ce qui la rend particulièrement adaptée pour des applications web et des systèmes de gestion de données volumineuses.

Dans le cadre de ce TP, CouchDB sera utilisée pour stocker les données de notre matrice de liens entre pages web. Chaque page pourra être représentée par un document dans CouchDB, où les lignes de la matrice (représentant les pages) seront stockées sous forme de documents avec des vecteurs de poids.

Partie 2 : Concept de MapReduce
MapReduce est un modèle de programmation pour traiter de grandes quantités de données de manière distribuée. Il se compose de deux étapes principales :

Map : Chaque élément de données est traité indépendamment. Dans cette étape, une fonction de mappage prend en entrée des données et génère des paires clé-valeur intermédiaires.

Reduce : Les paires clé-valeur produites par l'étape de mappage sont regroupées par clé, et une fonction de réduction les traite pour produire le résultat final.

MapReduce est particulièrement utile pour les calculs sur de grandes quantités de données réparties sur plusieurs machines.

Partie 3 : Représentation de la matrice de liens
La matrice M est une matrice carrée de dimension N×N représentant les liens entre les pages web. Chaque élément 
𝑀
𝑖
𝑗
M 
ij
​
  de la matrice représente le poids du lien entre la page 
𝑃
𝑖
P 
i
​
  et la page 
𝑃
𝑗
P 
j
​
 .

Nous allons utiliser CouchDB pour modéliser cette matrice sous forme de documents structurés. Chaque document représente une page 
𝑃
𝑖
P 
i
​
 , et contiendra un vecteur de taille N représentant les liens sortants de cette page vers les autres pages. Par exemple, un document pour la page 
𝑃
𝑖
P 
i
​
  pourrait ressembler à ceci :

json
Copier
Modifier
{
  "_id": "page_i",
  "links": [0.2, 0.3, 0.5, ..., 0.1]
}
Dans cet exemple, le tableau links contient les poids des liens sortants de la page 
𝑃
𝑖
P 
i
​
  vers les autres pages. Chaque document représente donc une ligne de la matrice 
𝑀
M.

Partie 4 : Traitement MapReduce pour calculer la norme des vecteurs
Le calcul de la norme d'un vecteur 
𝑉
=
(
𝑣
1
,
𝑣
2
,
.
.
.
,
𝑣
𝑁
)
V=(v 
1
​
 ,v 
2
​
 ,...,v 
N
​
 ) est donné par la formule :

∣
∣
𝑉
∣
∣
=
𝑣
1
2
+
𝑣
2
2
+
.
.
.
+
𝑣
𝑁
2
∣∣V∣∣= 
v 
1
2
​
 +v 
2
2
​
 +...+v 
N
2
​
 
​
 
L'idée ici est de calculer la norme pour chaque vecteur (chaque ligne de la matrice) représentée dans CouchDB. Le traitement MapReduce se déroule en deux étapes :

Map : Pour chaque document de la collection 
𝐶
C (représentant une page 
𝑃
𝑖
P 
i
​
 ), la fonction de map extrait le vecteur de poids links et émet une paire clé-valeur où la clé est l'ID de la page et la valeur est le vecteur de poids.

Exemple de fonction Map :

javascript
Copier
Modifier
function(doc) {
  emit(doc._id, doc.links);
}
Reduce : La fonction de réduction prend les valeurs (vecteurs de poids) pour chaque clé (page) et calcule la norme de chaque vecteur. La norme est calculée comme la racine carrée de la somme des carrés des éléments du vecteur.

Exemple de fonction Reduce :

javascript
Copier
Modifier
function(keys, values, rereduce) {
  if (rereduce) {
    return values.reduce((acc, val) => acc + val, 0);
  } else {
    return Math.sqrt(values.reduce((sum, v) => sum + Math.pow(v, 2), 0));
  }
}
Partie 5 : Calcul du produit matrice-vecteur
Nous souhaitons calculer le produit de la matrice 
𝑀
M avec un vecteur 
𝑊
=
(
𝑤
1
,
𝑤
2
,
.
.
.
,
𝑤
𝑁
)
W=(w 
1
​
 ,w 
2
​
 ,...,w 
N
​
 ), où chaque élément 
𝑊
𝑗
W 
j
​
  est un poids associé à la page 
𝑃
𝑗
P 
j
​
 . Le résultat de ce produit est un vecteur 
𝜑
φ où chaque élément 
𝜑
𝑖
φ 
i
​
  est défini par :

𝜑
𝑖
=
∑
𝑗
=
1
𝑁
𝑀
𝑖
𝑗
𝑊
𝑗
φ 
i
​
 = 
j=1
∑
N
​
 M 
ij
​
 W 
j
​
 
Le vecteur 
𝑊
W est disponible en mémoire et accessible par toutes les fonctions Map ou Reduce. Le traitement MapReduce pour effectuer ce calcul se déroule comme suit :

Map : Pour chaque document représentant une page 
𝑃
𝑖
P 
i
​
 , la fonction de mappage va associer les éléments de la ligne de la matrice 
𝑀
𝑖
M 
i
​
  avec les valeurs correspondantes du vecteur 
𝑊
W.

Exemple de fonction Map :

javascript
Copier
Modifier
function(doc) {
  var W = [w1, w2, ..., wN];  // Vecteur W accessible ici
  var result = doc.links.map(function(link, index) {
    return link * W[index];
  });
  emit(doc._id, result);
}
Reduce : La fonction de réduction additionne les produits obtenus pour chaque page, pour calculer le produit de la matrice avec le vecteur.

Exemple de fonction Reduce :

javascript
Copier
Modifier
function(keys, values, rereduce) {
  if (rereduce) {
    return values.reduce((acc, val) => acc + val, 0);
  } else {
    return values.reduce((acc, val) => acc + val, 0);
  }
}
