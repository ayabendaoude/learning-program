transaction database :
une bd pour enregistrer et gérer des transactions de manière fiable et cohérente.

Une transaction = une suite d'opérations qui doivent être exécutées complètement ou pas du tout.

exemple : virement de a vers b

la bd doit retirer du compte a + ajouter au compte b
si les 2 reussis donc transaction validé . si 1 echoue tout est annulé

ACID :
les bd respectent


A  : atomicité ( atomicity )

La transaction est tout ou rien.

Retirer 100 DH → OK
Ajouter 100 DH → erreur

Alors la base fait un rollback :
Retirer 100 DH → annulé



C : cohérence ( consistency )

la bd doit rester dans un etat valide

Si un compte ne peut pas être négatif :

Compte A = 50 DH
Retirer 100 DH → interdit

La transaction échoue.

clé primaire doit etre unique ...


I : isolation

Si plusieurs utilisateurs travaillent en même temps, leurs transactions ne doivent pas se perturber.

Deux personnes modifient un stock en même temps.

Chaque transaction est isolée des autres.

La base doit éviter les conflits.




D : durabilité ( durability )

Une fois la transaction validée (commit), les données sont définitivement enregistrées, même si le serveur crash.




    BEGIN TRANSACTION;
    
    UPDATE accounts
    SET balance = balance - 100
    WHERE id = 1;
    
    UPDATE accounts
    SET balance = balance + 100
    WHERE id = 2;
    
    COMMIT;

si erreur arrive ROLLBACK

OLTP

benefits : data accuracy / flexibility / speed 

optimisation : indexes / data model ...





en spring :
controller - service - repository / DAO - database


TRANSACTION TYPES :

1. global transactions ( distributed ) : plsrs ressources ( 1 app + 2 db + message queue ... )
2. local transactions : 1 seule ressource ( 1 app + 1 bd + 1 transaction manager ( repository ) )

on utilise un transaction manager distribué ( 2 phase commit )
Phase 1 : Prepare
Tous les systèmes disent : ready to commit ?
Phase 2 : Commit
Si tout est OK : commit everywhere
Sinon : rollback everywhere


ISOLATION LEVELS :
** default
** read_uncommitted
** read_committed
** repeatable_read
** serializable

TYPES DE GESTION DES TRANSACTIONS DANS SPRING :
1. Declarative transaction management ( @Transactional / xml )
2. Programmatic transaction management (le dev gere la transaction lui-meme )


avec PlatformTransactionManager

    TransactionStatus status =
    transactionManager.getTransaction(definition);
    
    try {
    
        withdraw();
        deposit();
    
        transactionManager.commit(status);
    
    } catch(Exception e) {
    
        transactionManager.rollback(status);
    
    }

AOP 
Proxy uses 
** Transaction interceptor which intercepts method calls.
** Platform transaction manager that handles transactions.


Spring creates proxies for all the classes annotated with @Transactional
** Transaction aspect : C’est la partie AOP de Spring qui détecte @Transactional.

Exemple :

@Transactional
public void transferMoney() {
withdraw();
deposit();
}

Quand cette méthode est appelée :
Transaction Aspect
↓
start transaction
↓
execute method
↓
commit / rollback
Donc il décide :
faut-il créer une transaction ?
faut-il commit ?
faut-il rollback ?



** Transaction manager : C’est le composant qui exécute réellement les opérations de transaction.
Il fournit les opérations :
begin()
commit()
rollback()
Donc le Transaction Aspect appelle le Transaction Manager.



** Entity manager proxy : En JPA, on utilise normalement un EntityManager pour parler à la base.


** Persistence context proxy : Le Persistence Context est un espace mémoire géré par l’EntityManager.


    @Transactional : spring crée un proxy autour de la méthode
    start transaction
    execute method
    commit / rollback
    
    
    @Transactional ( propagation = Propagation.REQUIRED )
    //Code will always run in a transaction
    
    @Transactional ( propagation = Propagation.REQUIRES_NEW )
    //Code will always run in a new transaction
    
    @Transactional ( propagation = Propagation.NEVER )
    //Method shouldn’t be run within a transaction
    @Transactional(isolation = Isolation.READ_UNCOMMITTED)
    //Allows dirty reads
    @Transactional(isolation = Isolation.READ_COMMITTED)
    //Does not allow dirty reads
    @Transactional(isolation = Isolation.REPEATABLE_READ)
    //Result always the same if row read twice
    @Transactional(isolation = Isolation.SERIALIZABLE)
    //Performs all transactions in a sequence
    @Transactional( timeout=5 )
    //Timeout for the operation wrapped by the transaction
    @Transactional( read-only = true )
    /**
    => Transactions don’t write back to database
    => Optimizes data access
    => Provides a hint to the persistence provider
    => Only relevant inside a transaction
    */




































































































