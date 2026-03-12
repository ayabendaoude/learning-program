Application
↓
Spring Data JPA
↓
JPA API
↓
Hibernate
↓
Database



1-- JPA ( Jakarta Persistence ) :
JPA est une spécification Java pour l’ORM.

Elle définit :
**des annotations (@Entity, @Id, etc.)
**des interfaces (ex: EntityManager)
**le fonctionnement du persistence context

Mais JPA ne contient pas l’implémentation.
Donc JPA ne peut pas fonctionner seule.



2-- Hibernate ORM (Object Relational Mapping) :
implémentation de JPA

**génère le SQL
**parle avec la base
**gère le persistence context

Donc :
JPA = contrat
Hibernate = implémentation

JPA/Hibernate permettent de mapper une classe Java à une table SQL.



3-- Spring Data:
un framework qui simplifie l’accès aux données.
Il supporte plusieurs bases : MongoDB Cassandra JPA ...



4-- Spring Data JPA :
module de Spring Data qui utilise JPA.

Il permet de créer des repositories automatiquement.

Spring Data JPA fournit JpaRepository

    public interface UserRepository extends JpaRepository<User, Long> {
    
    }

Requêtes personnalisées avec @Query.

    @Query("SELECT u FROM User u WHERE u.email = :email")
    User findByEmail(@Param("email") String email);

Annotations essentielles JPA : @Entity @Table @Id @GeneratedValue @Column



RELATIONS ENTRE ENTITES :

1-- OneToMany / ManyToOne
User → Orders ( 1 user peut avoir plsrs commandes )

table users
id (PK)
username

table orders
id (PK)
product
user_id (FK)

    @Entity
    public class User {
    
        @Id
        @GeneratedValue
        private Long id;
    
        private String username;
    
        @OneToMany(mappedBy = "user")
        private List<Order> orders;
    
    }

    @Entity
    public class Order {
    
        @Id
        @GeneratedValue
        private Long id;
    
        private String product;
    
        @ManyToOne
        @JoinColumn(name = "user_id")
        private User user;
    
    }

2-- OneToOne
User → Profile ( Chaque utilisateur a un seul profil )

table users
id (PK)
username

table profiles
id (PK)
bio
user_id (FK)

    @Entity
    public class User {
    
        @Id
        @GeneratedValue
        private Long id;
    
        private String username;
    
        @OneToOne(mappedBy = "user")
        private Profile profile;
    
    }
    
    @Entity
    public class Profile {
    
        @Id
        @GeneratedValue
        private Long id;
    
        private String bio;
    
        @OneToOne
        @JoinColumn(name = "user_id")
        private User user;
    
    }


3-- ManyToMany
Students ↔ Courses
Un étudiant peut suivre plusieurs cours
Un cours peut avoir plusieurs étudiants.

table students
id (PK)
name

table courses
id (PK)
title

table student_course
student_id (FK)
course_id (FK)

    @Entity
    public class Student {
    
        @Id
        @GeneratedValue
        private Long id;
    
        private String name;
    
        @ManyToMany
        @JoinTable(
            name = "student_course",
            joinColumns = @JoinColumn(name = "student_id"),
            inverseJoinColumns = @JoinColumn(name = "course_id")
        )
        private List<Course> courses;
    
    }
    
    @Entity
    public class Course {
    
        @Id
        @GeneratedValue
        private Long id;
    
        private String title;
    
        @ManyToMany(mappedBy = "courses")
        private List<Student> students;
    
    }

Remarque si 1..* , dev doit utiliser des contraintes supplémentaires 

exemples :

    @OneToMany(mappedBy = "user")
    @NotEmpty // ou @Size(min = 1)
    private List<Order> orders;

    @ManyToOne(optional = false)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;


EAGER VS LAZY LOADING :

** Avec EAGER, Hibernate charge immédiatement les relations.

    @OneToMany(fetch = FetchType.EAGER)
    private List<Order> orders;

quand on charge un User → on charge aussi ses Orders

    User user = repository.findById(1).get();

Hibernate exécute :

    SELECT * FROM users WHERE id = 1;
    SELECT * FROM orders WHERE user_id = 1;

** Avec LAZY, Hibernate charge les relations seulement si on les utilise.

    @OneToMany(fetch = FetchType.EAGER)
    private List<Order> orders;

    User user = repository.findById(1).get();

Hibernate fait seulement :

    SELECT * FROM users WHERE id = 1;

Les orders ne sont pas encore chargées.

Si plus tard on fait :

    user.getOrders();

Alors Hibernate exécute :

    SELECT * FROM orders WHERE user_id = 1;

==> on préfère LAZY pour performance


Le problème N+1 Query :

avec LAZY , si dev fait

    users.get(0).getOrders()
    users.get(1).getOrders()
    users.get(2).getOrders()

Hibernate exécute :

    SELECT * FROM orders WHERE user_id=1
    SELECT * FROM orders WHERE user_id=2
    SELECT * FROM orders WHERE user_id=3

CASCADE :

    @ManyToOne(optional = false, cascade = CascadeType.PERSIST)
    @JoinColumn(name = "teacher_id")
    private Teacher teacher;
    
    Teacher teacher = new Teacher();
    
    Course course = new Course("C# 101");
    course.setTeacher(teacher);
    
    entityManager.persist(course);



Hibernate fera automatiquement :
persist teacher
persist course
( pas besoin de faire persist teacher avant de faire persist course )



Les types de Cascade

Cascade	        effet
PERSIST	        sauvegarder aussi l'objet lié
MERGE	        mettre à jour aussi
REMOVE	        supprimer aussi
REFRESH	        rafraîchir aussi
DETACH	        détacher aussi
ALL	            tous les précédents




















