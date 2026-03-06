5 principes SOLID :

1 -- single responsibility principle ( SPA )

Une classe doit avoir une seule responsabilité (une seule raison de changer).

Si une classe fait plusieurs tâches différentes, alors :
elle devient difficile à maintenir
difficile à tester
difficile à modifier

FAUX !!!

    class UserService {
    
        public void createUser(User user){
            // logique création utilisateur
        }
    
        public void saveUserToDatabase(User user){
            // sauvegarde base de données
        }
    
        public void sendWelcomeEmail(User user){
            // envoyer email
        }
    }

CORRECT !!!

    class UserService {
        public void createUser(User user){}
    }
    class UserRepository {
        public void save(User user){}
    }
    class EmailService {
        public void sendWelcomeEmail(User user){}
    }

2 -- Open / Closed Principle ( OCP )

Les entités logicielles (classes, modules, fonctions) doivent être ouvertes à l’extension mais fermées à la modification.

On peut ajouter du comportement
Sans modifier le code existant

FAUX !!!

    class PaymentService {
    
        public void pay(String type){
    
            if(type.equals("card")){
                System.out.println("Pay with card");
            }
    
            else if(type.equals("paypal")){
                System.out.println("Pay with PayPal");
            }
    
        }
    }

CORRECT !!!

    interface Payment {
        void pay();
    }
    
    class CardPayment implements Payment {
    
        public void pay(){
            System.out.println("Pay with card");
        }
    }
    
    class PaypalPayment implements Payment {
    
        public void pay(){
            System.out.println("Pay with PayPal");
        }
    }

3 -- Liskov Substitution Principle (LSP)

Une classe enfant doit pouvoir remplacer sa classe parent sans casser le programme.
Si une classe enfant change le comportement attendu, alors l’héritage est mauvais.

Si B hérite de A, alors B doit pouvoir remplacer A partout sans problème.

FAUX !!!

    class Bird {
        void fly(){
            System.out.println("Flying");
        }
    }
    
    class Penguin extends Bird {
    
        void fly(){
            throw new RuntimeException("Penguins can't fly");
        }
    
    }

    List<Bird> birds = List.of(
        new Sparrow(),
        new Eagle(),
        new Penguin()
    );
    
    for(Bird bird : birds){
        bird.fly();
    }
    
    void makeBirdFly(Bird bird){
        bird.fly();
    }
    
    makeBirdFly(new Bird());
    makeBirdFly(new Sparrow());
    makeBirdFly(new Penguin());

Quand une classe hérite d’une autre, elle promet : "Je me comporte comme mon parent."
Donc si :Bird.fly() existe, tous les enfants doivent pouvoir voler.
Sinon l’héritage est incorrect.

CORRECT !!!

    class Bird {}
    
    class FlyingBird extends Bird {
        void fly(){}
    }
    
    class Sparrow extends FlyingBird {}
    class Eagle extends FlyingBird {}
    class Penguin extends Bird {}

    void makeBirdFly(FlyingBird bird){
        bird.fly();
    }



FAUX !!!

    class Payment {
        void refund(){
            System.out.println("Refund");
        }
    }
    
    class CashPayment extends Payment {
    
        void refund(){
            throw new UnsupportedOperationException();
        }
    
    }
    Payment p = new CashPayment();
    p.refund();

CORRECT !!!

    interface Payment {}
    
    interface Refundable {
        void refund();
    }
    
    class CardPayment implements Payment, Refundable {

        public void refund(){}
    }
    
    class CashPayment implements Payment {}

aussi exemple de Square / Rectangle

4-- Interface Segregation Principle (ISP)

Une classe ne doit pas être forcée d’implémenter des méthodes qu’elle n’utilise pas.
petites interfaces spécialisées : oui
grosses interfaces avec plein de méthodes : non

FAUX !

    interface Worker {
    
        void work();
    
        void eat();
    
        void sleep();
    }

    class HumanWorker implements Worker {
    
        public void work(){}
    
        public void eat(){}
    
        public void sleep(){}
    
    }
    
    class RobotWorker implements Worker {
    
        public void work(){}
    
        public void eat(){
            throw new UnsupportedOperationException();
        }
    
        public void sleep(){
            throw new UnsupportedOperationException();
        }
    
    }

CORRECT !!!

    interface Workable {
        void work();
    }
    
    interface Eatable {
        void eat();
    }
    
    interface Sleepable {
        void sleep();
    }
    
    class HumanWorker implements Workable, Eatable, Sleepable {
    
        public void work(){}
    
        public void eat(){}
    
        public void sleep(){}
    
    }
    
    class RobotWorker implements Workable {
    
        public void work(){}
    
    }

5 -- Dependency Inversion Principle (DIP)
The Dependency Inversion Principle (DIP) states that we should depend on abstractions (interfaces and abstract classes) instead of concrete implementations (classes). The abstractions should not depend on details; instead, the details should depend on abstractions.

FAUX !!!

    class MySQLDatabase {
    
        public void connect(){
            System.out.println("Connecting to MySQL");
        }
    
    }
    
    class UserService {
    
        MySQLDatabase database = new MySQLDatabase();
    
        public void saveUser(){
            database.connect();
        }
    
    }

Problème : UserService dépend directement de MySQLDatabase
Si on change la base PostgreSQL , on doit modifier UserService.

CORRECT !!!

    interface Database {
        void connect();
    }
    
    class MySQLDatabase implements Database {
    
        public void connect(){
            System.out.println("MySQL connection");
        }
    
    }
    
    class UserService {
    
        private Database database;
    
        public UserService(Database database){
            this.database = database;
        }
    
        public void saveUser(){
            database.connect();
        }
    
    }

    Database db = new MySQLDatabase();
    Database db = new PostgreSQLDatabase();
    UserService service = new UserService(db);

