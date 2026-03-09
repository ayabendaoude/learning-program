architecture de junit5 :
JUnit Platform ( execute les tests )
JUnit Jupiter ( framework de test , contient les annotations / api de test / extensions )
JUnit Vintage ( permet d executer anciens tests junit3 et junit4 )

Annotations :
@Test ( méthode de test )
@BeforeEach ( exécuté avant chaque test, avant @Before )
@AfterEach ( executé apres test, avant @After )
@BeforeAll ( executé 1 seule fois avant tous les tests, avant @BeforeClass )
@AfterAll ( executé 1 seule fois apres tous les tests, avant @AfterClass )
@DisplayName ( donne un nom lisible au test )
@Tag ( catégoriser les tests )
@Nested ( organiser les tests en groupes )
@Disabled ( desactiver un test, avant @Ignore )
@ExtendWith ( ajoute des extensions à JUnit )
@TestFactory ( génére plusieurs tests dynamiques )

exemples :

    @Test
    void testAdd(){}
    
    @BeforeEach
    void setup(){}
    
    @BeforeAll // must be static
    static void setup(){}

    @AfterEach
    void setup(){}

    @AfterAll // must be static
    static void setup(){}

    @DisplayName("Test addition de deux nombres")
    @Test
    void testAdd(){}
    
    @Tag("integration")
    @Test
    void testDatabase(){}

    @Disabled("Not implemented yet")
    @Test
    void testNotReady(){}



sans Nested :

    class UserServiceTest {
    
        @Test
        void createUser_valid(){}
    
        @Test
        void createUser_invalid(){}
    
        @Test
        void deleteUser_success(){}
    
        @Test
        void deleteUser_notFound(){}
    
    }



avec Nested :

    class UserServiceTest {
    
        @Nested
        class CreateUserTests {
    
            @Test
            void createUser_valid(){}
    
            @Test
            void createUser_invalid(){}
    
        }
    
        @Nested
        class DeleteUserTests {
    
            @Test
            void deleteUser_success(){}
    
            @Test
            void deleteUser_notFound(){}
    
        }
    
    }

extendwith : exemple avec mock ( objet simulé fake qui remplace une vraie dépendance)

    @ExtendWith(MockitoExtension.class)
    class UserServiceTest {
    
        @Mock // crée un faux objet
        UserRepository repository; 
    
        @InjectMocks // crée l'objet réel à tester et lui injecte les mocks
        UserService service;
    
        @Test
        void testCreateUser(){
    
            service.createUser("Aya");
    
        }
    
    }

sans testfactory :

    @Test
    void testAdd1(){
        assertEquals(4, 2 + 2);
    }
    
    @Test
    void testAdd2(){
        assertEquals(5, 2 + 3);
    }

avec testfactory :

    @TestFactory
    Collection<DynamicTest> dynamicTests() {
    
        return List.of(
            DynamicTest.dynamicTest("2+2=4", () -> assertEquals(4, 2+2)), // nom du test + code du test
            DynamicTest.dynamicTest("2+3=5", () -> assertEquals(5, 2+3))
        );
    
    }


Assertions :
les assertions dans le package : org.junit.jupiter.api.Assertions
expressions lambda

assertEquals(4, 2 + 2);

assertTrue(condition, message to show if false / can be lambda fct)

assertAll(
"nom du groupe",
() -> assertion1,
() -> assertion2,
() -> assertion3
); // regroupe plsrs assertions dans un seul test , et les executentes toutes même si certains échouent


exemples : 

    @Test
    void testSum() {
    
        int sum = 1 + 2 + 3;
    
        assertTrue(sum > 5, "Sum should be greater than 5");
    }

sans AssertAll :

    @Test
    void testNumbers() {
    
        int[] numbers = {0, 1, 2, 3, 4};
    
        assertEquals(1, numbers[0]); // faux -> test echoue -> les autres assertions jamais axecutées
        assertEquals(3, numbers[3]);
        assertEquals(1, numbers[4]);
    }

avec AssertAll :

    @Test
    void testNumbers() {
    
        int[] numbers = {0, 1, 2, 3, 4};
    
        assertAll("numbers",
            () -> assertEquals(1, numbers[0]),
            () -> assertEquals(3, numbers[3]),
            () -> assertEquals(1, numbers[4])
        );
    }


Assumptions :
servent à executer un test seulement si une condition est vraie .
lance execption TestAbortedException et le test est simplement ignoré

assumeTrue(condition) // test continue si la condition est vraie.
assumeFalse(condition) // test continue si la condition est false.
assumingThat(condition, assertion) // exécute seulement une partie du test si la condition est vraie. et ne skippe pas le reste

exemples :

    @Test
    void trueAssumption() {
    
        assumeTrue(5 > 1);
    
        assertEquals(5 + 2, 7);
    }

    @Test
    void falseAssumption() {
    
        assumeFalse(5 < 1);
    
        assertEquals(5 + 2, 7);
    }

    @Test
    void assumptionThat() {
    
        String someString = "Just a string";
    
        assumingThat(
            someString.equals("Just a string"),
            () -> assertEquals(2 + 2, 4)
        );
    
    }


Exception Handling :
Dans certains cas, on veut vérifier que le programme lance une erreur correctement.

assertThrows(ExceptionType.class, () -> {
    code à tester
});

exemples :

    @Test
    void shouldThrowException() {
    
        Throwable exception = assertThrows(
            UnsupportedOperationException.class,
            () -> {
                throw new UnsupportedOperationException("Not supported");
            }
        );
    
        assertEquals("Not supported", exception.getMessage());
    }

    @Test
    void assertThrowsException() {
    
        String str = null;
    
        assertThrows(
            IllegalArgumentException.class,
            () -> {
                Integer.valueOf(str); // NumberFormatException hérite de IllegalArgumentException donc le test passe
            }
        );
    }


Suites :
regroupe plusieurs classes de tests et de les exécuter ensemble au lieu d'executer 1 par 1 .

    @Suite
    @SelectPackages("com.baeldung") // exécute tous les tests d’un package
    @ExcludePackages("com.baeldung.suites")
    public class AllUnitTest {} // ne contient pas les tests , sert juste a regrouper

    @Suite
    @SelectClasses({ // selectionne des classes spécifiques 
    AssertionTest.class,
    AssumptionTest.class,
    ExceptionTest.class
    })
    public class AllUnitTest {}


Dynamic tests :
générer des tests automatiquement pendant l’exécution (runtime). le nbr de tests dépend par exemple de la liste , fichier , bd , api ...
on utilise @TestFactory pour generer plsrs tests et la methodes doit retourner : stream / collection / iterable / iterator

    List<String> in  = ["hello", "world"];
    List<String> out = ["bonjour", "monde"];
    
    @TestFactory
    Stream<DynamicTest> translateDynamicTestsFromStream() {
    
        return in.stream()
            .map(word ->
                DynamicTest.dynamicTest( // nom du test + code tu test
                    "Test translate " + word,
                    () -> {
                        int id = in.indexOf(word);
                        assertEquals(out.get(id), translate(word));
                    }
                )
            );
    }









Junit 6 :

@ParameterizedTest : Permet d’exécuter le même test avec plusieurs valeurs.

    @ParameterizedTest
    @ValueSource(ints = {1,2,3})
    void testNumbers(int number) {
        assertTrue(number > 0);
    } // junit va executer testNumbers(1) et testNumbers(2) et testNumbers(3) donc 3 tests sont générés auto

@RepeatedTest : Permet de répéter un test plusieurs fois.

    @RepeatedTest(5)
    void testRepeat() {
        System.out.println("Running test");
    }

@TestTemplate

