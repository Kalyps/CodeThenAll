# SQL
## Description
C'est un serveur de base de donnée  
Le langage utilisé est le SQL *Structured Query Language*, c'est le langage de base de donné le plus utilisé.  
**[mySQL](https://www.mysql.com/)** est détenu par Oracle, c'est un produit commercial avec une version libre **MySQL Community Edition**.  
Depuis 2009 son créateur original à créé **[MariaDB](https://mariadb.com/)** pour poursuivre le développement libre sous licence GPL.

La dernière version de mySQL est la 5.7 .

## SQL
Le langage SQL est une norme depuis 1986. Depuis 2011 c'est la norme **ISO/CEI 9075:2011**.
L'accès est contrôlé par utilisateur avec des droits différents (*Lecture, écriture, *).  
**[SQLite](https://www.sqlite.org/index.html)** permet d'utiliser des requette SQL sans avoir besoin de serveur de base de donnée.

## mcd
Modèle conception données
C'est un schéma de fonctionnement de la base de données.
On liste pour chaque tables les champs quelles contiennent. Puis on lie les tables ensembles avec des verbes d'actions.  
On note les cardinalités.
0..1 : 0 ou 1 (peut être null)  
0..n : 0 ou plus (peut être null)  
1..1 : 1 et 1 seule  
1..n : 1 ou plus  

Table intermédiaire

## Les principales commandes
### Manipulation de données
Ces commandes accèdent ou modifient le contenu de la base de données

- SELECT
- UPDATE
- INSERT
- DELETE

### Définition de données
Ces commande modifient la structure de la base de données

- CREATE
- ALTER
- DROP

## Comparaison logique
Le NULL n'est pas comparé, la comparaison d'une valeur NULL renvois NULL et non pas TRUE ou FALSE.  

 - AND : opérateur et prioritaire sur le OR
 - OR : ou

## Clés primaire
La clés primaire est toujours un entier. Elle sert à identifier les lignes uniques. Elle sert uniquement au fonctionnement interne.

	ALTER TABLE voiture ADD COLUMN id int PRIMARY KEY AUTO_INCREMENT UNIQUE;

## Clés étrangère
Vas chercher une valeur dans une autre table.

	ALTER TABLE voiture ADD COLUMN idconducteur int NOT NULL REFERENCES conducteur(id) ;
	ALTER TABLE voiture ADD FOREIGN KEY (idconducteur) REFERENCES conducteur(id);

## ligne de commandes

	mysql -u phpmyadmin -p

## liens utiles
* [hostinger.fr](https://www.hostinger.fr/tutoriels/creer-un-utilisateur-mysql/)
* [openclassrooms.com](https://openclassrooms.com/courses/administrez-vos-bases-de-donnees-avec-mysql/creation-d-une-base-de-donnees)


## Exemples
### Exercice 1
Créer deux tables : Voiture (année, modèle, marque, couleur, immatriculation) et conducteur(nom, prénom, sexe, date permis)

	CREATE TABLE IF NOT EXISTS voiture (
		annee year NOT NULL, 
		modele varchar(50) NOT NULL,
		marque varchar(30) NOT NULL,
		couleur varchar(30),
		immatriculation char(7) NOT NULL
	);

	CREATE TABLE IF NOT EXISTS conducteur ( nom varchar(100) NOT NULL, prenom varchar(100) NOT NULL, sexe enum('femme', 'homme', 'autre'), date_permis DATE NOT NULL );

- varchar() : nombre de caractères maxi
- char() : nombre de caractères, complété par des espaces si moins

Remplir les tables

	INSERT INTO  voiture VALUES(
		2006,
		'Yaris Luna Pack',
		'Toyota',
		'gris',
		'DL456FR'
	),(
		2007,
		'407 berline pack Feline',
		'Peugeot',
		'Champagne',
		'DQ408zn'
	),(
	   2017,
		'Duster j''yconnait rien',
		'Dacia',
		'Gris Comette',
		'MA978EK'
	);

	INSERT INTO conducteur VALUES ('Pruliere','Jean', 'autre', '2006-05-01'),
	('Braga', 'Julien', NULL, '2013-06-15'),
	('Thuaire', 'Magali', 'femme', '1999-12-12');

lire des données:

	SELECT modele,annee,id FROM `voiture` 

Créer une clef:

	ALTER TABLE voiture ADD COLUMN id int PRIMARY KEY AUTO_INCREMENT UNIQUE;

Rendre la valeur d'une colonne unique. Une seule personne avec le même nom et prénom.

	ALTER TABLE voiture ADD UNIQUE (immatriculation)
	ALTER TABLE conducteur ADD UNIQUE (`nom`, `prenom`)

Ajouter une entrée

	INSERT INTO `voiture`(`annee`, `modele`, `marque`, `couleur`, `immatriculation`) VALUES (2001,'panda 4x4','fiat', 'vert claire', 'fr456ez')
	INSERT INTO `voiture`(`annee`, `modele`, `marque`, `immatriculation` ) VALUES (2005,'206+','Peugeot','de345re')

Lire les entrée croisé:  
Mauvaise méthode:

	SELECT conducteur.nom, conducteur.prenom, voiture.marque, voiture.modele FROM conducteur, voiture WHERE conducteur.id = voiture.idconducteur;

Bonne méthode

	SELECT conducteur.nom, conducteur.prenom, voiture.marque, voiture.modele FROM conducteur JOIN voiture ON conducteur.id = voiture.idconducteur;


### Exercice 2
Créer
les écoles du réseaux wf3
les sessions
les élève
les profs


	CREATE TABLE IF NOT EXISTS ecole (
		id int AUTO_INCREMENT PRIMARY KEY,
		lieu varchar(100) NOT NULL,
		date_debut date NOT NULL
	);

	CREATE TABLE IF NOT EXISTS promo (
		id int AUTO_INCREMENT PRIMARY KEY,
		idecole int ,
		nom varchar(100) NOT NULL,
		date_debut date NOT NULL,
		FOREIGN KEY (idecole) REFERENCES ecole(id)
	);

	CREATE TABLE IF NOT EXISTS eleve (
		id int AUTO_INCREMENT PRIMARY KEY,
		idpromo int ,
		nom varchar(100) NOT NULL,
		prenom varchar(100) NOT NULL,
		date_debut date NOT NULL,
		FOREIGN KEY (idpromo) REFERENCES promo(id)
	);

	CREATE TABLE IF NOT EXISTS prof (
		id int AUTO_INCREMENT PRIMARY KEY,
		nom varchar(100) NOT NULL,
		prenom varchar(100) NOT NULL,
		specialitee varchar(100) NOT NULL
	);

	CREATE TABLE IF NOT EXISTS cour (
		id int AUTO_INCREMENT PRIMARY KEY,
		idprof int ,
		idpromo int ,
		nom varchar(100) NOT NULL,
		date date NOT NULL,
		FOREIGN KEY (idpromo) REFERENCES promo(id),
		FOREIGN KEY (idprof) REFERENCES prof(id)
	);

avec numérotation aléatoire:  

	UPDATE `promo` SET `idecole`=FLOOR(1 + RAND() * 3) WHERE 1



	INSERT INTO `cour`(`idprof`, `idpromo`, `nom`, `date`) VALUES 
	(FLOOR(1 + RAND() * SELECT COUNT(*) FROM `prof` ), FLOOR(1 + RAND() * SELECT COUNT(*) FROM `promo`), , )

Affiche les élèves, promos, Ecoles, le cour et la date et le nom du prof en en fonction du nom du prof

	SELECT eleve.nom, eleve.prenom, promo.nom, ecole.lieu, cour.nom, cour.date, prof.nom 
	FROM eleve JOIN prof JOIN promo JOIN cour JOIN ecole 
	WHERE eleve.idpromo = promo.id AND prof.id = cour.idprof AND cour.idpromo = promo.id AND promo.idecole = ecole.id AND prof.nom='Prune'

