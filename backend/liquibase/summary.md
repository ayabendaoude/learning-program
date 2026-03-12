Liquibase est un outil de database migration.
Son rôle : gérer les modifications de la base de données de manière versionnée et automatisée
Il permet de : créer des tables + modifier des colonnes + ajouter des index + supprimer des tables

tout cela avec un historique des changements.

2 concepts

** changelogs and changesets
** tracking tables : record successfull changes

dev write changes in changelog to add / remove schema changes ( sql json xml yaml )
changes should be stored in a vcs

changelog contains changesets ( units of change executed on a db schema : create  table / add index / drop column )

tracking tables are used to track , version and deploy db schema changes

2 types of tracking tables :
** databasechangelog : track each changeset by id + author + file of changeset
** databasechangeloglock : locks other dev out to no make updates to the same db schema at the same time ( avoid overriding )

liquibase allows dev to track , rollback, migrate db states to other dbs with ease


Liquibase ships with more than 40 commands for managing database changes.

Update Commands – deploy changes to the database.
Rollback Commands – rollback previously deployed changes.
Snapshot Commands – capture the current database state and save it to a changelog.
Diff Commands – compare database snapshots and/or live databases.
Status Commands – see the status of changes – deployed or undeployed.
Quality Check Commands – automated quality checks help you find and fix problems early.
Flow Commands – portable, platform-independent Liquibase workflows that can run anywhere.
Utility Commands – manage your Liquibase changelog and tracking files.


State-based deployments : on applique les changements un par un dans l’ordre ( Chaque modification est une migration. )
migration-based deployments : on ne stocke pas les étapes. On stocke seulement : l’état final du schéma

