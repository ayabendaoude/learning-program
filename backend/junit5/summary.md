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
@Nested (  )
@Disabled ( desactiver un test, avant @Ignore )
@ExtendWith (  )

exemples :

    @Test
    void testAdd(){}
    
    @BeforeEach
    void setup(){}
    
    @DisplayName("Test addition de deux nombres")
    @Test
    void testAdd(){}
    
    @Tag("integration")
    @Test
    void testDatabase(){}

    @Disabled
    @Test
    void testNotReady(){}
















