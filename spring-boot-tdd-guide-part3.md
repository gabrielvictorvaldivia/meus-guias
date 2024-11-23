## Padrões e Anti-padrões em Testes

### Padrões de Teste

#### 1. Builder Pattern para Objetos de Teste
```java
@Builder
public class UserTestBuilder {
    private Long id;
    private String name;
    private String email;
    private LocalDateTime createdAt;
    
    public static UserTestBuilder aUser() {
        return UserTestBuilder.builder()
            .id(1L)
            .name("John Doe")
            .email("john@example.com")
            .createdAt(LocalDateTime.now());
    }
    
    public User build() {
        return new User(id, name, email, createdAt);
    }
    
    public UserTestBuilder withCustomEmail(String email) {
        this.email = email;
        return this;
    }
}

// Uso
@Test
void shouldCreateUser() {
    User user = UserTestBuilder.aUser()
        .withCustomEmail("custom@example.com")
        .build();
}
```

#### 2. Object Mother Pattern
```java
public class UserMother {
    public static User aRegularUser() {
        return User.builder()
            .id(1L)
            .name("Regular User")
            .role(Role.USER)
            .build();
    }
    
    public static User anAdmin() {
        return User.builder()
            .id(2L)
            .name("Admin User")
            .role(Role.ADMIN)
            .build();
    }
    
    public static User aDeletedUser() {
        return User.builder()
            .id(3L)
            .name("Deleted User")
            .deletedAt(LocalDateTime.now())
            .build();
    }
}
```

#### 3. Factory Pattern para Mocks
```java
public class MockFactory {
    public static UserRepository createUserRepository() {
        UserRepository repository = mock(UserRepository.class);
        when(repository.findById(anyLong())).thenReturn(Optional.of(UserMother.aRegularUser()));
        when(repository.save(any())).thenAnswer(invocation -> invocation.getArgument(0));
        return repository;
    }
    
    public static AuthenticationService createAuthService() {
        AuthenticationService authService = mock(AuthenticationService.class);
        when(authService.getCurrentUser()).thenReturn(UserMother.aRegularUser());
        return authService;
    }
}
```

### Anti-padrões e Como Evitá-los

#### 1. Testes Frágeis
```java
// ❌ Anti-padrão: Teste dependente de dados específicos
@Test
void shouldFindUserById() {
    assertEquals("John", userService.findById(1L).getName());
}

// ✅ Padrão correto: Teste com dados controlados
@Test
void shouldFindUserById() {
    // Arrange
    User expectedUser = UserMother.aRegularUser();
    when(userRepository.findById(1L)).thenReturn(Optional.of(expectedUser));
    
    // Act
    User actualUser = userService.findById(1L);
    
    // Assert
    assertEquals(expectedUser.getName(), actualUser.getName());
}
```

#### 2. Testes Acoplados
```java
// ❌ Anti-padrão: Testes dependentes entre si
@Test
void shouldCreateAndThenUpdateUser() {
    User user = userService.create(userDTO);
    user.setName("Updated");
    User updatedUser = userService.update(user);
    assertEquals("Updated", updatedUser.getName());
}

// ✅ Padrão correto: Testes independentes
@Test
void shouldCreateUser() {
    User user = userService.create(userDTO);
    assertNotNull(user.getId());
}

@Test
void shouldUpdateUser() {
    User existingUser = UserMother.aRegularUser();
    when(userRepository.findById(existingUser.getId()))
        .thenReturn(Optional.of(existingUser));
    
    existingUser.setName("Updated");
    User updatedUser = userService.update(existingUser);
    assertEquals("Updated", updatedUser.getName());
}
```

#### 3. Testes Lentos
```java
// ❌ Anti-padrão: Teste com setup pesado
@Test
void shouldProcessLargeDataset() {
    List<User> users = createManyUsers(1000); // Cria 1000 usuários
    List<UserDTO> results = userService.processUsers(users);
    assertEquals(1000, results.size());
}

// ✅ Padrão correto: Teste focado e eficiente
@Test
void shouldProcessUsers() {
    List<User> users = Arrays.asList(
        UserMother.aRegularUser(),
        UserMother.anAdmin()
    );
    List<UserDTO> results = userService.processUsers(users);
    assertEquals(2, results.size());
}
```

## Testes de Performance

### JMeter Integration
```java
@Test
void performLoadTest() {
    // Configuração do JMeter
    StandardJMeterEngine jmeter = new StandardJMeterEngine();
    
    // Criar plano de teste
    TestPlan testPlan = new TestPlan("Create User Load Test");
    
    // Configurar grupo de threads
    ThreadGroup threadGroup = new ThreadGroup();
    threadGroup.setNumThreads(100);
    threadGroup.setRampUp(10);
    threadGroup.setDuration(60);
    
    // Adicionar sampler HTTP
    HTTPSampler httpSampler = new HTTPSampler();
    httpSampler.setDomain("localhost");
    httpSampler.setPort(8080);
    httpSampler.setPath("/api/users");
    httpSampler.setMethod("POST");
    
    // Executar teste
    jmeter.run();
}
```

### Gatling Tests
```scala
class UserSimulation extends Simulation {
  val httpProtocol = http
    .baseUrl("http://localhost:8080")
    .acceptHeader("application/json")
    
  val scn = scenario("Create Users")
    .exec(http("create_user")
      .post("/api/users")
      .body(StringBody("""{"name":"Test User","email":"test@example.com"}"""))
      .asJson
      .check(status.is(201)))
    .pause(1)
    
  setUp(
    scn.inject(
      rampUsers(100).during(10.seconds),
      constantUsersPerSec(10).during(1.minute)
    )
  ).protocols(httpProtocol)
}
```

### Spring Boot Actuator para Métricas
```java
@Test
void shouldMonitorEndpointPerformance() {
    // Configurar métricas
    MeterRegistry registry = new SimpleMeterRegistry();
    
    Timer timer = Timer.builder("api.user.creation")
        .description("Time taken to create user")
        .register(registry);
    
    // Executar operação com medição
    timer.record(() -> {
        userService.createUser(userDTO);
    });
    
    // Verificar métricas
    assertTrue(timer.count() > 0);
    assertTrue(timer.totalTime(TimeUnit.MILLISECONDS) < 1000);
}
```

### Testes de Carga com K6
```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
    vus: 100,
    duration: '1m',
};

export default function () {
    const payload = JSON.stringify({
        name: 'Test User',
        email: 'test@example.com',
    });
    
    const params = {
        headers: {
            'Content-Type': 'application/json',
        },
    };
    
    const res = http.post('http://localhost:8080/api/users', payload, params);
    
    check(res, {
        'status is 201': (r) => r.status === 201,
        'response time < 200ms': (r) => r.timings.duration < 200,
    });
    
    sleep(1);
}
```

### Métricas de Performance

#### 1. Tempo de Resposta
```java
@Test
void shouldMeetResponseTimeRequirements() {
    long startTime = System.currentTimeMillis();
    
    ResponseEntity<UserDTO> response = restTemplate.getForEntity("/api/users/1", UserDTO.class);
    
    long endTime = System.currentTimeMillis();
    long duration = endTime - startTime;
    
    assertTrue(duration < 100, "Response time should be less than 100ms");
    assertEquals(HttpStatus.OK, response.getStatusCode());
}
```

#### 2. Throughput
```java
@Test
void shouldHandleMultipleRequestsConcurrently() {
    int numberOfRequests = 100;
    ExecutorService executor = Executors.newFixedThreadPool(10);
    CountDownLatch latch = new CountDownLatch(numberOfRequests);
    AtomicInteger successCount = new AtomicInteger(0);
    
    for (int i = 0; i < numberOfRequests; i++) {
        executor.submit(() -> {
            try {
                ResponseEntity<String> response = restTemplate.getForEntity("/api/users", String.class);
                if (response.getStatusCode() == HttpStatus.OK) {
                    successCount.incrementAndGet();
                }
            } finally {
                latch.countDown();
            }
        });
    }
    
    latch.await(30, TimeUnit.SECONDS);
    assertTrue(successCount.get() > numberOfRequests * 0.95);
}
```
