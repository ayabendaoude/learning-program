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

    GRANT <privilege>[ou ALL] ON <no_objet> TO <nom_usager> [ou PUBLIC][WITH GRANT OPTION] 

    REVOKE <privilege>[ou ALL] ON <no_objet> FROM <nom_usager> [ou PUBLIC] 

    CREATE ROLE <nom_du_role>

    GRANT <privileges> ON <nom_objet>TO <nom_role> 

    GRANT <nom_role> TO <nom_usager>

TRANSACTIONS :

    COMMIT : confirmer une modif ( INSERT UPDATE DELETE ) ou une transaction ( série de commandes de manipulation de données depuis le dernier COMMIT ) sur la bd
    
    ROLLBACK [TO nom_save_point] : annule une transaction COMMIT
    
    SAVEPOINT nom_savepoint : fixe des points de sauvegarde

Autres commandes : SHOW / DESCRIBE / DESC / CONCAT /

PLSQL : composé de 3 blocs

** bloc déclaratif ( optionnelle ) : déclaration des variables , commence par DECLARE

** bloc de controle / execution ( obligatoire ) : commence par BEGIN et se termine par END; /

** bloc  dees exceptions ( optionnelle ) : commence par EXCEPTION

un programme plsql peut etre anonyme ou stocké dans le serveur sous forme de procedure , fonction ou trigger. dans tous les cas , le code est COMPILE

exemple bloc declaration :

    DECLARE
    NOM VARCHAR2(30);
    SALAIRE NUMBER(8,2);
    DATEEMBAUCHE DATE;
    CHEK BOOLEAN;
    PRENOM VARCHAR2(30) := 'PATOCHE'; 
    PI CONSTANT NUMBER(6,5):= 3.14159;

exemple %TYPE 

    Salaire_MIN NUMBER(7,2);
    Salaire_MAX Salaire_MIN%TYPE;
    Acteur VARCHAR2(30);
    Realisateur Acteur%TYPE :='Spielberg';
    NOM  ETUDIANTS.NOMETUDIANT%TYPE; // permet de déclarer la variable NOM avec le même type que la variable NOMETUDIANT de la table ETUDIANT


exemple %ROWTYPE

    SET SERVEROUTPUT ON; // obligtoire  pour afficher les messages généré par le code plsql notamment avec DBMS_OUTPUT.PUT_LINE
    DECLARE
    ENRETU ETUDIANTS2%ROWTYPE; // Le type %ROWTYPE, permet de déclarer une variable de type ligne d’une table.
    NUM  ETUDIANTS.NUMAD%TYPE:=12;
    BEGIN
    SELECT NOM, PRENOM INTO ENRETU FROM ETUDIANTS WHERE NUMAD =NUM;
    DBMS_OUTPUT.PUT_LINE('le nom est '|| enretu.nom || 'le prenom est ' ||enretu.prenom);
    END;

exemple RECORD

    DECLARE  
    TYPE ENRE IS RECORD // le type record permet de déclarer une variable de type enregistrements
    (
    NOM1 ETUDIANTS.NOM%TYPE,
    PRENOM1 VARCHAR2(20)
    );
    numad1 NUMBER :=12;
    ENR ENRE;
    BEGIN
    SELECT NOM, PRENOM INTO ENR.NOM1,ENR.PRENOM1
    FROM ETUDIANTS
    WHERE numad = numad1;
    BMS_OUTPUT.PUT_LINE('le nom est '|| ENR.nom1 || 'le prenom est ' ||ENR.prenom1);
    END;

CURSOR :

permet de parcourir les résultats d'une requete ligne par ligne
quand une requete sql retourne plusieurs lignes , oracle stocke le resulat dans une zone mémoire appelée cursor , et le curseur lit ligne par ligne

En PL/SQL :
SELECT INTO fonctionne seulement pour 1 ligne.
Si la requête retourne plusieurs lignes, on doit utiliser un curseur.

    CURSOR resultat IS
    SELECT nom, prenom
    FROM etudiant
    WHERE nom LIKE '%POIT%';

3 etapes pour utiliser un curseur

** OPEN : ouvre le curseur et exécute la requête. Oracle récupère les lignes et les place dans la mémoire.

** FETCH : lit 1 ligne à la fois et passe à la suivante pour chaque fetch.

** CLOSE : ferme le curseur et libère mémoire

exemple 

    SET SERVEROUTPUT ON;
    
    DECLARE
    CURSOR c_etudiants IS
    SELECT nom, prenom
    FROM etudiant;
    
    v_nom user1.etudiant.nom%TYPE; // schema = utilisateur de la base , chaque user possede ses tables
    v_prenom etudiant.prenom%TYPE;
    
    BEGIN
    
    OPEN c_etudiants;
    
    LOOP
    FETCH c_etudiants INTO v_nom, v_prenom;
    
          EXIT WHEN c_etudiants%NOTFOUND;
    
          DBMS_OUTPUT.PUT_LINE(v_nom || ' ' || v_prenom);
    
    END LOOP;
    
    CLOSE c_etudiants;
    
    END;
    /

attributs des curseurs : 

    %FOUND ( Retourne TRUE si le FETCH a trouvé une ligne. ) 
    
    %NOTFOUND 
    
    %ISOPEN ( Vérifie si le curseur est ouvert. ) 
    
    %ROWCOUNT ( donne le nombre de lignes deja lues ) 

CURSEUR DYNAMIQUE :

Un curseur normal est lié à une seule requête SQL. Ce curseur exécutera toujours cette requête.

    CURSOR c_etudiants IS
    SELECT nom, prenom
    FROM etudiants; // declaration

Un curseur dynamique n’est pas lié à une seule requête. La requête peut changer au moment de l’exécution.

    TYPE nom_type IS REF CURSOR; // declaration

exemple 

    SET SERVEROUTPUT ON;
    
    DECLARE
    TYPE etudiant_cursor IS REF CURSOR;
    
    c etudiant_cursor;
    
    v_nom ETUDIANTS.NOM%TYPE;
    
    choix NUMBER(1);
    
    BEGIN
    
    IF choix = 1 THEN
    OPEN c FOR SELECT * FROM etudiants;
    ELSE
    OPEN c FOR SELECT * FROM professeurs;
    END IF;
    
    LOOP
    FETCH c_etudiants INTO v_nom;
    
          EXIT WHEN c_etudiants%NOTFOUND;
    
          DBMS_OUTPUT.PUT_LINE(v_nom);
    
    END LOOP;
    
    CLOSE c_etudiants;
    
    END;
    /

2 -- Section d'execution / controle

**ALTERNATIVE :

IF THEN END IF

    DECLARE  
    CHOIX NUMBER ;
    BEGIN
    IF CHOIX =1 THEN  UPDATE ETUDIANTS SET CYCLE =2 WHERE NUMAD=12;
    END IF;
    END; 

IF‐ THEN ‐ ELSE END IF

    DECLARE  
    CHOIX NUMBER ;
    BEGIN
    IF CHOIX =1 THEN  UPDATE ETUDIANTS SET CYCLE =2 WHERE NUMAD=12;
    ELSE DELETE FROM ETUDIANTS WHERE NUMAD =12;
    END IF;
    END;

IF‐ THEN ‐ ELSIF END IF

    DECLARE  
    CHOIX NUMBER ;
    BEGIN
    IF CHOIX =1 THEN  UPDATE ETUDIANTS SET CYCLE =2 WHERE NUMAD=12;
    ELSIF CHOIX =2 THEN UPDATE ETUDIANTS SET CYCLE =2;
    ELSIF CHOIX =3 THEN INSERT INTO ETUDIANTS(NUMAD, NOM)
    VALUES(13,'PATOCHE');
    ELSE DELETE FROM ETUDIANTS WHERE NUMAD =12;
    END IF;
    END; 

CASE  WHEN

    CREATE OR REPLACE PROCEDURE MiseAJour(CHOIX IN NUMBER) AS
    BEGIN
    CASE CHOIX
    WHEN 1 THEN INSERT INTO employes (nom, salaire )VALUES('pataoche',28000);
    COMMIT;
    WHEN 2 THEN UPDATE employes SET salaire = salaire +100 where
    numemp =1480;
    COMMIT;
    ELSE dbms_output.put_line('pas bon choix');
    END CASE;
    END;

** REPETITIVE

LOOP Sequence_instructions EXIT WHEN condition END LOOP 

    SET SERVEROUTPUT ON;
    DECLARE
    Credit NUMBER :=0;
    BEGIN
    LOOP
    Credit :=Credit + 1;
    IF Credit> 3 THEN
    EXIT;
    END IF;
    END LOOP;
    DBMS_OUTPUT.PUT_LINE ('Credit : ' || TO_CHAR(Credit));
    END; 

WHILE condition LOOP instructions END LOOP 

    SET SERVEROUTPUT ON;
    DECLARE
    I NUMBER:=1;
    BEGIN
    WHILE I < 10 LOOP
    I:= I+1;
    DBMS_OUTPUT.PUT_LINE (TO_CHAR(I));
    END LOOP;
    END;

FOR  compteur  IN  [REVERSE] valeurs LOOP sequencesInstructions END LOOP 

    SET  SERVEROUTPUT ON;
    BEGIN
    FOR i IN  1..3 LOOP
    DBMS_OUTPUT.PUT_LINE (TO_CHAR(i));
    END LOOP;
    END; 

PROCEDURES STOCKEES :

c un pg plsql enregistré dans la bd . avantages : code stocké / mois de repetitioons ...

    CREATE OR REPLACE PROCEDURE nom_procedure
    (
    param1 [IN | OUT | IN OUT] type,
    param2 [IN | OUT | IN OUT] type
    )
    IS
        declarations_variables // on n’utilise pas DECLARE
    BEGIN
        bloc_plsql
    END;
    /

exemple : 

    CREATE OR REPLACE PROCEDURE INSERTION
    (
    PNOM IN EMPLOYES.NOM%TYPE,
    PPRN IN EMPLOYES.PRENOM%TYPE
    )
    IS
    BEGIN
    INSERT INTO EMPLOYES(NOM, PRENOM)
    VALUES (PNOM, PPRN);
    END;
    /
    
    EXEC INSERTION('POITRAS','REMI');

FONCTIONS :

pg dans plsql stocké dans bd qui retourne obligatoirement une valeur fonction = procédure + RETURN

différence procedure vs fonction :

    ne retourne pas obligatoirement une valeur	            retourne toujours une valeur
    appelée avec EXEC	                                    utilisée dans une requête SQL
    utilisée pour actions (INSERT, UPDATE…)	                utilisée pour calculs

EXEC procedure_test();

SELECT fonction_test() FROM DUAL;

    CREATE OR REPLACE FUNCTION nom_fonction
    (
    param1 [IN | OUT | IN OUT] type,
    param2 [IN | OUT | IN OUT] type
    )
    RETURN type_retour
    IS
        variables
    BEGIN
        bloc_plsql
        RETURN valeur;
    END;
    /

PACKAGE :

Un package est un objet de la base de données qui encapsule d’autres objets (procédures, fonctions)

2 parties :

1. La partie déclaration ou spécification du package: dans cette partie sont déclarées
   les variables globales, les curseurs, les procédures et les fonctions.
2. La partie corps du programme: dans cette partie, sont définies les procédures et
   les fonctions et les curseurs.


    CREATE OR REPLACE
    PACKAGE gestionetudiants AS
    -- DECLARATION DE LA VARIABLE GLOBALE DE TYPE CURSEUR DYNAMIQUE
    TYPE REQUETE IS REF CURSOR; ---DÉCLARATION DES PROCÉDURES ET FONCTIONS ---- LES PROCÉDURES
    PROCEDURE INSERTION(PNUMAD IN NUMBER, PNOM IN VARCHAR2,PPRENOM
    VARCHAR2);
    PROCEDURE SUPPRESSION(PNUMAD IN ETUDIANTS.NUMAD%TYPE);
    PROCEDURE SELECTION (PCODEPRG IN ETUDIANTS.CODEPRG%TYPE, RESULTAT OUT
    REQUETE); -- LES FONCTIONS
    FUNCTION SELECTION2 (PCODEPRG IN ETUDIANTS.CODEPRG%TYPE) RETURN REQUETE;
    FUNCTION TOTAL RETURN NUMBER;
    END gestionetudiants;

TRIGGERS :

Un trigger est une procédure PL/SQL qui s’exécute automatiquement lorsqu’un événement se produit sur une table. Les événements les plus fréquents sont les opérations DML ( INSERT , UPDATE , DELETE )

    CREATE OR REPLACE TRIGGER nom_trigger
    BEFORE | AFTER
    INSERT OR UPDATE OR DELETE
    ON nom_table
    [FOR EACH ROW]
    [WHEN condition]
    BEGIN
    bloc PL/SQL
    END;
    /

type de triggers :
1 - trigger global ( statement-level ): n'utilise pas FOR EACH ROW 

    CREATE OR REPLACE TRIGGER ctrlmiseajour
    BEFORE INSERT OR DELETE OR UPDATE ON EMPLOYES
    DECLARE
        MESSAGE EXCEPTION;
    BEGIN
        IF (TO_CHAR(SYSDATE,'DY')='SAT' OR TO_CHAR(SYSDATE,'DY')='SUN') THEN
        RAISE MESSAGE;
    END IF;
    
    EXCEPTION
        WHEN MESSAGE THEN
            RAISE_APPLICATION_ERROR(-20324,'Modification interdite le weekend');
    END;
    /

2- trigger ligne (row level) : s execute pour chaque ligne modifiée , on utilise FOR EACH ROW

    CREATE OR REPLACE TRIGGER empins
    BEFORE INSERT ON EMPLOYES
    FOR EACH ROW
    BEGIN
        IF :NEW.NUMEMP IS NULL THEN
            SELECT SQEMP2.NEXTVAL INTO :NEW.NUMEMP FROM DUAL;
        END IF;
    END;
    /

ALTER TRIGGER nomTrigger ENABLE / DISABLE : Un trigger doit être activé pour fonctionner.

ALTER TABLE nom_table DISABLE ALL TRIGGERS : desactive tous les triggers de la table

DROP TRIGGER nomTrigger;

Les prédicats INSERTING / UPDATING / DELETING

    CREATE TRIGGER testTrigger
    BEFORE INSERT OR UPDATE ON employe
    BEGIN
    
    IF INSERTING THEN
    DBMS_OUTPUT.PUT_LINE('Insertion');
    
    ELSIF UPDATING(salaire) THEN // update d un colonne salaire
    DBMS_OUTPUT.PUT_LINE('Modification');
    
    END IF;
    
    END;
    /

