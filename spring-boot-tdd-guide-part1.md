# Guia Definitivo de TDD com Spring Boot

## Índice
1. [Introdução ao TDD](#introdução-ao-tdd)
2. [Configuração do Ambiente](#configuração-do-ambiente)
3. [Tipos de Testes](#tipos-de-testes)
4. [Testes de Unidade](#testes-de-unidade)
5. [Testes de Integração](#testes-de-integração)
6. [Testes de API (Endpoints)](#testes-de-api)
7. [Testes de Segurança](#testes-de-segurança)
8. [Cobertura de Testes](#cobertura-de-testes)
9. [Boas Práticas](#boas-práticas)

## Introdução ao TDD

### O que é TDD?
Test-Driven Development (TDD) é uma metodologia de desenvolvimento que se baseia no princípio de escrever testes antes do código de produção. O ciclo do TDD consiste em:

1. Escrever um teste que falha (Red)
2. Implementar o código mínimo para passar no teste (Green)
3. Refatorar o código mantendo o teste passando (Refactor)

### Por que usar TDD no Spring Boot?
- Maior confiabilidade do código
- Documentação viva através dos testes
- Design mais limpo e modular
- Facilita refatoração
- Redução de bugs em produção

## Configuração do Ambiente

### Dependências Essenciais
```xml
<dependencies>
    <!-- Spring Boot Starter Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    
    <!-- JUnit 5 -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <scope>test</scope>
    </dependency>
    
    <!-- Mockito -->
    <dependency>
        <groupId>org.mockito</groupId>
        <artifactId>mockito-junit-jupiter</artifactId>
        <scope>test</scope>
    </dependency>
    
    <!-- REST Assured -->
    <dependency>
        <groupId>io.rest-assured</groupId>
        <artifactId>rest-assured</artifactId>
        <scope>test</scope>
    </dependency>
    
    <!-- TestContainers -->
    <dependency>
        <groupId>org.testcontainers</groupId>
        <artifactId>junit-jupiter</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### Estrutura de Diretórios Recomendada
```
src/
├── main/
│   └── java/
│       └── com/example/project/
│           ├── controllers/
│           ├── services/
│           ├── repositories/
│           └── domain/
└── test/
    └── java/
        └── com/example/project/
            ├── unit/
            │   ├── controllers/
            │   ├── services/
            │   └── domain/
            ├── integration/
            └── e2e/
```

## Tipos de Testes

### Pirâmide de Testes
A pirâmide de testes é um conceito que ajuda a entender a proporção ideal de diferentes tipos de testes em sua aplicação:

```mermaid
pyramid
    title Pirâmide de Testes
    "E2E" : 10
    "Integração" : 30
    "Unidade" : 60
```

- **Testes de Unidade**: Base da pirâmide, devem ser a maioria
- **Testes de Integração**: Camada intermediária
- **Testes E2E**: Topo da pirâmide, menor quantidade

## Testes de Unidade

### Componentes Testáveis
- Services
- Controllers
- Repositories
- Domain Objects
- Utils

### Exemplo de Teste de Service
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void shouldCreateUser() {
        // Arrange
        UserDTO userDTO = new UserDTO("John", "john@email.com");
        User user = new User(1L, "John", "john@email.com");
        when(userRepository.save(any())).thenReturn(user);
        
        // Act
        User created = userService.createUser(userDTO);
        
        // Assert
        assertNotNull(created);
        assertEquals(user.getId(), created.getId());
        assertEquals(user.getName(), created.getName());
        verify(userRepository).save(any());
    }
}
```

### Melhores Práticas para Testes de Unidade
1. Use nomes descritivos para os testes
2. Siga o padrão Arrange-Act-Assert
3. Um assert por teste
4. Isole dependências usando mocks
5. Teste casos de sucesso e falha

## Testes de Integração

### Configuração com @SpringBootTest
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserIntegrationTest {
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldCreateAndRetrieveUser() {
        // Arrange
        UserDTO userDTO = new UserDTO("John", "john@email.com");
        
        // Act
        ResponseEntity<User> createResponse = restTemplate.postForEntity("/api/users", userDTO, User.class);
        
        // Assert
        assertEquals(HttpStatus.CREATED, createResponse.getStatusCode());
        assertNotNull(createResponse.getBody());
        
        // Verify in database
        Optional<User> savedUser = userRepository.findById(createResponse.getBody().getId());
        assertTrue(savedUser.isPresent());
    }
}
```

### Uso do TestContainers
```java
@Testcontainers
@SpringBootTest
class DatabaseIntegrationTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:14")
        .withDatabaseName("testdb")
        .withUsername("test")
        .withPassword("test");
    
    @DynamicPropertySource
    static void properties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
    
    @Test
    void contextLoads() {
        assertTrue(postgres.isRunning());
    }
}
```

## Testes de API

### Testes com REST Assured
```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
class UserControllerTest {
    @LocalServerPort
    private int port;
    
    private String baseUrl;
    
    @BeforeEach
    void setUp() {
        baseUrl = "http://localhost:" + port + "/api";
    }
    
    @Test
    void shouldCreateUser() {
        UserDTO user = new UserDTO("John", "john@email.com");
        
        given()
            .contentType(ContentType.JSON)
            .body(user)
        .when()
            .post(baseUrl + "/users")
        .then()
            .statusCode(201)
            .body("name", equalTo("John"))
            .body("email", equalTo("john@email.com"));
    }
}
```

### Mock MVC para Testes de Controller
```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void shouldGetUser() throws Exception {
        User user = new User(1L, "John", "john@email.com");
        when(userService.getUser(1L)).thenReturn(user);
        
        mockMvc.perform(get("/api/users/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("John"))
            .andExpect(jsonPath("$.email").value("john@email.com"));
    }
}
```
