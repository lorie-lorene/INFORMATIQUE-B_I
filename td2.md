### creation des tables

CREATE TABLE emprunt(idLivre integer,FOREIGN KEY (idLivre) REFERENCES livre(idLivre), idEmprunt int,idabonne integer,FOREIGN KEY (idabonne) REFERENCES abonne(idabonne),date_emprunt date);

CREATE TABLE abonne(idabonne integer,nom varchar(50),telephone int,code_postal int);

CREATE TABLE livre(idlivre integer,isbn int,titre varchar(50),auteur var char(50),anne_edition int,prix int);
# faire de meme pour les autres tables
exo1:
3)
a-le nombre de films produits par genre:

SELECT idgenre, COUNT(*) AS films
FROM Film
GROUP BY idgenre;
--- 
SELECT GenreFilm.libelle, COUNT(*) AS films
FROM Film INNER JOIN GenreFilm ON Film.idgenre=GenreFilm.idGenre
GROUP BY GenreFilm.libelle;
---
b-pour chaque genre et chaque année, le nombre de films produits dans ce genre et
depuis cette année:

SELECT GenreFilm.libelle,annee, COUNT(*) AS films
FROM Film INNER JOIN GenreFilm ON Film.idgenre=GenreFilm.idGenre
GROUP BY GenreFilm.libelle,annee;
---
c-les personnes ayant joué dans un film lorsqu’il avait moins de 21 ans:

SELECT Personne.nom, Film.titre, 2023-YEAR(dateNaissance) as age
FROM Personne, roleFilm, Film
WHERE Personne.idpersonne=roleFilm.idpersonne
AND roleFilm.idfilm=Film.idfilm
AND 2023-YEAR(dateNaissance)<21;
---
d-les films qui n’ont aucun acteur de moins de 25 ans:

SELECT Film.idfilm, Film.titre
FROM Personne, roleFilm, Film
WHERE Personne.idpersonne=roleFilm.idpersonne
AND roleFilm.idfilm=Film.idfilm
AND 2023-YEAR(dateNaissance)>25
AND Film.idfilm NOT IN (SELECT Film.idfilm
FROM Personne, roleFilm, Film
WHERE Personne.idpersonne=roleFilm.idpersonne
AND roleFilm.idfilm=Film.idfilm
AND 2023-YEAR(dateNaissance)<=25
GROUP BY Film.idfilm);

g. le nombre d’acteurs par film:
SELECT Film.titre, COUNT(*) AS nombre_acteur
FROM `roleFilm`
INNER JOIN Film
WHERE roleFilm.idfilm=Film.idfilm
GROUP BY(roleFilm.idfilm);
---

h. le nombre moyen d’acteurs par film:
SELECT Film.titre, COUNT(*) AS nombre_acteur
FROM `roleFilm`
INNER JOIN Film
WHERE roleFilm.idfilm=Film.idfilm
GROUP BY(roleFilm.idfilm)
HAVING AVG(nombre_acteur);
---

i. le genre de film ayant le plus grand nombre d’acteurs :
# nombre d'acteur par genre
SELECT GenreFilm.libelle, COUNT(*) AS nombre_acteur
FROM `roleFilm`
INNER JOIN Film,GenreFilm
WHERE roleFilm.idfilm=Film.idfilm
AND Film.idgenre=GenreFilm.idGenre
GROUP BY(GenreFilm.libelle);
---

exercice 2
1. Afficher le titre et l’auteur de tous les livres:

SELECT titre , auteur
FROM livre;
---
2. Afficher le nom, le code postal et le numéro de téléphone des abonnés sur Paris (code
postaux débutants par 75):

SELECT code_postal,telephone,nom
FROM abonne
WHERE code_postal LIKE '75%';
---
3. Afficher, sans doublon, l’isbn et l’auteur des livres selon l’ordre alphabétique des auteurs et
des titres:

SELECT auteur,isbn,titre
FROM `livre`
GROUP BY  auteur;
---
4. Afficher le titre des livres empruntés par l’abonné n°669:
SELECT emprunt.idLivre, livre.titre
FROM emprunt
INNER JOIN livre,abonne   
WHERE livre.idLivre=emprunt.idLivre
And emprunt.idabonne=abonne.idabonne
AND emprunt.idabonne='669';
---
5. Afficher la liste des emprunts sur la ville de Brest (code postal 29200):
SELECT * 
FROM emprunt
INNER JOIN abonne   
WHERE emprunt.idabonne=abonne.idabonne
AND abonne.code_postal='29200';
---
6. Afficher le nom et le prénom des abonnés qui empruntent actuellement un exemplaire du
livre “Aline”:

SELECT abonne.nom
FROM emprunt
INNER JOIN livre,abonne
WHERE emprunt.idLivre=livre.idLivre
AND emprunt.idabonne=abonne.idabonne
AND livre.titre='Aline';
---
7. Afficher le prix total de tous les livres possédés par bibliothèque:
SELECT SUM(prix)
FROM `livre`;
---
8. Afficher le nombre d’abonnés par code postal:
SELECT  code_postal ,COUNT(*)
FROM `abonne`
GROUP BY code_postal;
---
9. Afficher, par auteur, le nombre de livres (et non pas d’exemplaires) possédés par la
bibliothèque, qui ont été édités en 2017:
SELECT auteur , COUNT(*) AS nombre
FROM `livre`
WHERE annee_edition='2017'
GROUP BY(auteur );
---
10. Afficher le titre des livres de “Voltaire” présents en plus de 3 exemplaires:
SELECT  titre
FROM `livre`
WHERE auteur='Voltaire';