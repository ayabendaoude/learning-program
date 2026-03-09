elements de oracle :
** le noyeau
** dictionnaire de données
** SQL
** PL/SQL

3 types de commandes SQL 
** LDD : langage de définition des données pour créer / modifier un objet de la bd ( CREATE ALTER DROP MODIFY )
** LMD : langage de manipulation des données pour misa a jour / voir la data ( SELECT ALTER DROP MODIFY )
** LCD : langage de controle de data ( GRANT REVOKE )
** TCL : langage de transaction pour gérer les modifs à la bd ( COMMIT ROLLBACK)

objets d'oracle :
** tables
** vues
** user
** séquences ( générateur de numéro unique )
** synonymes ( autre nom pour objet table , vue , séquence , schema )
** procédures : programme plsql
** déclencheurs : procédure déclanchée avant toute modif ( TRIGGER )

LDD :

CREATE TABLE 

    CREATE TABLE nom_table (
        nom_colonne type_donnees
            [DEFAULT valeur]
            [CONSTRAINT nom_contrainte]
            [NOT NULL | UNIQUE | PRIMARY KEY | REFERENCES table(colonne)]
            [CHECK (condition)]
    );

NUMBER / CHAR(n) / VARCHAR2(n) / DATE / CLOB ( texte long ) 

    CREATE TABLE users (
        id NUMBER PRIMARY KEY,
        name VARCHAR2(100) NOT NULL,
        email VARCHAR2(100) UNIQUE,
        age NUMBER CHECK (age >= 18),
        status VARCHAR2(20) DEFAULT 'ACTIVE'
    );
    
    CREATE TABLE orders (
        id NUMBER PRIMARY KEY,
        customer_id NUMBER,
        amount NUMBER,
        CONSTRAINT fk_customer
            FOREIGN KEY (customer_id)
            REFERENCES customers(id)
            ON DELETE CASCADE
    );

ALTER TABLE :

ADD :

    ALTER TABLE Employes ADD (Salaire NUMBER (8,2));
    
    ALTER TABLE Employes ADD CONSTRAINT emppk PRIMARY KEY (NumEmp);

MODIFY :

    ALTER TABLE Employes MODIFY (nom NOT NULL);

ENABLE / DISABLE :

    ALTER TABLE Employes DISABLE Primary Key;

DROP :

    ALTER TABLE Employes DROP Primary Key;  
    
    ALTER TABLE Employes DROP CONSTRAINT emppk;
    
    ALTER TABLE Employes  DROP COLUMN nom;

    DROP TABLE personne; 


RENAME :

    ALTER TABLE Employes  RENAME COLUMN Salaire  TO SalaireEmp;

    RENAME <Ancien_nom_table> TO <Nouveau_nom>;


INDEX :

    CREATE [UNIQUE] INDEX Nom_Index ON Nom_Table(nomClonne) ;

INSERT :

    INSERT INTO <nom_de_table> VALUES (<liste de valeurs>); 
    
    INSERT INTO <nom_de_table>(<nom_de_colonne>) VALUES (<liste_de_valeurs>);

    INSERT INTO ETUDIANTS (NUMAD, NOM, PRENOM) VALUES (seq_etudiants.NEXTVAL, 'Ali', 'Karim');

SEQUENCE :

    CREATE SEQUENCE nom_sequence
    START WITH valeur_depart
    INCREMENT BY intervalle
    MINVALUE valeur_min
    MAXVALUE valeur_max
    [CYCLE | NOCYCLE];

    CREATE SEQUENCE seq_etudiants
    START WITH 1
    INCREMENT BY 1;

UPDATE :

    UPDATE <nom_de _table> SET <nom_de_colonne>=<nouvelle_valeur> WHERE <condition>; 

LIKE / IN / IS NULL / BETWEEN X AND Y / ANY / ALL / EXISTS  +++ NOT dans condition

DELETE :

    DELETE FROM <nom_de _table> WHERE <condition>;

SELECT :

    SELECT [DISTINCT] {* | colonne | expression [AS alias], ...}
    FROM table1
    [WHERE condition]
    [GROUP BY expression]
    [HAVING condition]
    [ORDER BY colonne | expression | alias [ASC | DESC]]
    [{UNION | INTERSECT | MINUS} autre_requete];

MAX / MIN / AVG / SUM / COUNT /

JOINTURES :

PRODUIT CARTESIEN :

    SELECT NOM, PRENOM, NOMPROG FROM ETUDIANTS,PROGRAMME; 

JOINTURE SIMPLE :

    SELECT NOM, PRENOM, NOMPROG FROM ETUDIANTS E INNER JOIN PROGRAMME P ON E.CODEPRG =P.CODEPRG;

JOINTURE EXTERNE :

    SELECT NOM, PRENOM, NOMPROG FROM ETUDIANTS E LEFT OUTER JOIN PROGRAMME P ON E.CODEPRG=P.CODEPRG; 

CONNECT BY :

    SELECT NUM, NOM, NUMRES, LEVEL
    FROM EMPLOYES
    START WITH NUM = 10
    CONNECT BY PRIOR NUM = NUMRES;

table DUAL (dummy / permet d executer une requete sql sans utiliser une vraie table )

LES FONCTIONS SQL :

modification des chaines de caracteres :

    LENGTH
    UPPER
    LOWER
    INITCAP( 1ere lettre en majuscule )
    || ( concatenation )
    LPAD('7',5,'0') : ajoute des caractere a gauche d un texte / donne 00007
    RPAD
    LTRIM : supprime les caractères à gauche
    RTRIM
    DECODE(expression, valeur1, resultat1, valeur2, resultat2, resultat_default) : if/else DECODE(sexe,'M','Homme','F','Femme')
    SUBSTR(texte, position, longueur) : extraire une partie d un texte SELECT SUBSTR('OracleSQL',1,6) donne Oracle
    REPLACE(texte, ancien, nouveau) : remplace un texte par un autre
    INSTR(texte, mot) : Cherche la position d’un texte dans une chaîne.

dates :

    SYSDATE / ADD_MONTHS / NEXT_DAY / LAST_DAY / MONTHS_BETWEEN

nombres :

    ROUND / TRUNC / CEIL / FLOOR / POWER / SQRT / MOD

conversion :

    TO_DATE / TO_CHAR / TO_NUMBER

LES VUES :

    CREATE [OR REPLACE ][FORCE] VIEW <nom_de_la_vue> AS <sou_requête> [WITH CHECK OPTION]

    DROP VIEW nom_de_vue

    RENAME ancien_nom TO nouveau_NOM

SYNONYMES :

    CREATE [PUBLIC] SYNONYM <nom_du_sysnonyme> FOR <nom_objet> 

    DROP [PUBLIC] SYNONYM <nom_synonyme>

PRIVILEGES :






