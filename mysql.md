# MySQL

##### Les chevrons (>>>) indiquent que la commande doit être exécutée dans un shell MySQL.

#### USERS

Voir les utilisateurs et leurs privilèges :

```sql
>>> SELECT User, Select_priv, Insert_priv, Create_priv, Update_priv FROM user;
```

Créer un utilisateur. :

```sql
>>> CREATE USER 'savio'@'192.168.1.%' IDENTIFIED BY 'm0nk';
```

Voir les privilèges d'un utilisateur :

```sql
>>> SHOW GRANTS FOR 'savio'@'192.168.1.%';
```

Attribuer des privilèges :

```sql
>>> GRANT SELECT ON editor.* TO 'savio'@'192.168.1.%';
```

Changer mot de passe :

```sql
>>> SET PASSWORD FOR 'user'@'localhost' = PASSWORD('mdp');
```

#### TABLES

Mettre une table en utf8 :

```sql
>>> ALTER TABLE Table CONVERT TO CHARACTER SET utf8;
```

Modifier une colonne :

```sql
>>> ALTER TABLE Table CHANGE ancien_nom nouveau_nom VARCHAR(25) DEFAULT NULL;
```

Renommer une table :

```sql
>>> RENAME TABLE Xxx TO Yyy;
```

Compter les enregistrements de 'table' :

```sql
>>> SELECT COUNT(*) FROM table;
```

#### JOINTURES

Assuming you're joining on columns with no duplicates, which is a very common case:
An inner join of A and B gives the result of A intersect B, i.e. the inner part of a Venn diagram intersection.
An outer join of A and B gives the results of A union B, i.e. the outer parts of a Venn diagram union.

**Examples :**
Suppose you have two tables, with a single column each, and data as follows:

```
A    B
-    -
1    3
2    4
3    5
4    6
```

Note that (1,2) are unique to A, (3,4) are common, and (5,6) are unique to B.

**INNER JOIN**
An inner join using either of the equivalent queries gives the intersection of the two tables, i.e. the two rows they have in common.

```sql
SELECT * FROM a INNER JOIN b ON a.a = b.b;
SELECT a.*, b.* FROM a,b WHERE a.a = b.b;

a | b
--+--
3 | 3
4 | 4
```

**LEFT OUTER JOIN**
A left outer join will give all rows in A, plus any common rows in B.

```sql
SELECT * FROM a LEFT OUTER JOIN b ON a.a = b.b;
SELECT a.*, b.*  FROM a,b WHERE a.a = b.b(+);

a |  b
--+-----
1 | null
2 | null
3 |    3
4 |    4
```

**RIGHT OUTER JOIN**
A right outer join will give all rows in B, plus any common rows in A.

```sql
SELECT * FROM a RIGHT OUTER JOIN b ON a.a = b.b;
SELECT a.*, b.*  FROM a,b WHERE a.a(+) = b.b;

a    |  b
-----+----
3    |  3
4    |  4
null |  5
null |  6
```

**FULL OUTER JOIN**
A full outer join will give you the union of A and B, i.e. all the rows in A and all the rows in B. If something in A doesn't have a corresponding datum in B, then the B portion is null, and vice versa.

```sql
SELECT * FROM a FULL OUTER JOIN b ON a.a = b.b;

a   |  b
-----+-----
1 | null
2 | null
3 |    3
4 |    4
null |    6
null |    5
```

#### UPDATE

Mettre à jour tous les enregistrements d'une table (ici `Objets`) en attribuant à un champ une valeur extraite d'une table jointe (ici `Interpretes_CD`) (on fait le INNER JOIN et ensuite le SET):

```sql
>>> UPDATE Objets AS o
INNER JOIN Interpretes_CD AS i
ON o.Interprete_CD = i.ID
SET o.Interprete = i.Interprete;
```

On peut faire une jointure avec une autre base. Ici nous allons mettre à jour le champ `commentaire_listing` de la table `abbaye.hotellerie_sejour` en récupérant ce même champ dans une ancienne table (`hotellerie.sejours_sejour`) et en nous basant sur l'ID :

```sql
>>> UPDATE abbaye.hotellerie_sejour AS a
INNER JOIN hotellerie.sejours_sejour AS h
ON a.id = h.id
SET a.commentaire_listing = h.commentaire_listing;
```

#### RECHERCHES

Avec le mot clé BINARY, la recherche devient sensible à la casse :

```sql
>>> SELECT nom, prenom FROM Personnes WHERE BINARY nom LIKE 'DE %';
```

GROUP BY avec COUNT(\*) (ici on compte le nombre de mails de chaque personne) :

```sql
>>> SELECT id_personne, COUNT(*) AS nb_mails FROM Mails GROUP BY id_personne ORDER BY nb_mails;
```

#### FIELDS

Tronquer un champ à droite (field, length, fill) :

```sql
>>> RPAD(text, 50, “.”);
```

#### FOREIGN KEYS

Créer une clé étrangère :

```sql
>>> ALTER TABLE <table1> ADD CONSTRAINT <fk_name> FOREIGN KEY (<column1>) REFERENCES <table2>(<column2>);
```

Voir les clés étrangères d'une table :

```sql
>>> SHOW CREATE TABLE <table>\G;
```

Supprimer une clé étrangère :

```sql
>>> ALTER TABLE <table> DROP FOREIGN KEY <fk_name>;
```

Voir les clés étrangères de toutes les tables :

```sql
>>> SELECT TABLE_NAME, COLUMN_NAME, CONSTRAINT_NAME, REFERENCED_TABLE_NAME, REFERENCED_COLUMN_NAME FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE WHERE CONSTRAINT_NAME LIKE "%fk_%";
```

#### OPÉRATIONS MATHÉMATIQUES

Cette commande fait, pour tous les Objets dont le champ Ref_TM commence par "L1", la somme des montants dans la table Charges et la divise par le champ Chiffre_tirage de la table Objets (le GROUP BY final ne sert à rien mais évite la levée d'une erreur) :

```sql
>>> SELECT o.Ref_TM, (SUM(c.montant) / o.Chiffre_tirage) AS prix_revient FROM Charges AS c INNER JOIN Objets AS o ON o.ID = c.ID_objet WHERE o.Ref_TM LIKE "L1%" GROUP BY o.ID;
```

#### JSON

On peut exporter un statement au format JSON grâce à la combinaison des commandes JSON_ARRAYAGG() et JSON_OBJECT() :

```sql
>>> SELECT JSON_ARRAYAGG(JSON_OBJECT(
'ref_tm', Ref_TM,
'ean', EAN,
'category', Cat_objet,
'title', Titre,
'sub_title', Sous_titre,
'author', Auteur
))
FROM Objets
WHERE ID = 159;
```

Il est possible d'imbriquer plusieurs JSON_ARRAYAGG() en cas de jointure de tables (ici Charges et Objets) :

```sql
>>> SELECT JSON_ARRAYAGG(JSON_OBJECT(
'ref_tm', o.Ref_TM,
'ean', o.EAN,
'category', o.Cat_objet,
'title', o.Titre,
'sub_title', o.Sous_titre,
'author', o.Auteur,
'charges', (SELECT JSON_ARRAYAGG(JSON_OBJECT('charge', c.Charge, 'montant', c.Montant)) FROM Charges AS c WHERE c.ID_objet = o.ID)
))
INTO OUTFILE '/var/lib/mysql-files/objects.json'
FROM Objets AS o;
```

#### PROCÉDURES STOCKÉES

Créer une procédure stockée (mettre à jour un verset de la polyglotte s'il existe, sinon le créer) :

```sql
>>> DELIMITER |
>>> CREATE PROCEDURE update_or_insert_hebrew(in_book VARCHAR(10), in_chapter INT, in_verse INT, in_txt TEXT)
>>> BEGIN
>>>     IF EXISTS(SELECT id FROM Verse WHERE book=in_book AND chapter=in_chapter AND verse=in_verse) THEN
>>>         UPDATE Verse SET txt_hebrew = in_txt WHERE book=in_book AND chapter=in_chapter AND verse=in_verse;
>>>     ELSE
>>>         INSERT INTO Verse(book, chapter, verse, txt_hebrew) VALUES(in_book, in_chapter, in_verse, in_txt);
>>>     END IF;
>>> END
>>> |
>>> DELIMITER ;
```

Appeler une procédure stockée :

```sql
>>> CALL update_or_insert_hebrew('Ex', 7, 30, 'test');
```

Supprimer une procédure stockée :

```sql
>>> DROP PROCEDURE update_or_insert_hebrew;
```

Voir toutes les procédures stockées :

```sql
>>> SELECT ROUTINE_TYPE, ROUTINE_NAME FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA='polyglotte';
```

#### EXPORTER DANS UN FICHIER

On n'a pas le droit d'exporter partout. Il faut d'abord rechercher dans quel répertoire on a le droit d'exporter :

```sql
>>> SHOW VARIABLES LIKE "secure_file_priv";
```

Ensuite exporter le statement dans ce répertoire (ici /var/lib/mysql-files/):

```sql
>>> SELECT Ref_TM, Titre, Sous_titre INTO OUTFILE '/var/lib/mysql-files/books.txt' FIELDS TERMINATED BY "|" FROM Objets WHERE Cat_objet = 1 ORDER BY Ref_TM;
```

#### DIVERS

Utile pour repérer une erreur (par ex. échec de création d’une clé étrangère) :

```sql
>>> SHOW ENGINE INNODB STATUS;
```

Exécuter un statement depuis le fichier file.sql sur la base my_database :

```sql
mysql my_database < file.sql
```
