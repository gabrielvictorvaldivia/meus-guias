## Implementação de Testes em CI/CD

### GitHub Actions Workflow
```yaml
name: Spring Boot CI/CD

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_DB: testdb
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'
        cache: maven
    
    - name: Run Tests
      run: |
        mvn test
        mvn jacoco:report
    
    - name: Upload Coverage Report
      uses: actions/upload-artifact@v3
      with:
        name: coverage-report
        path: target/site/jacoco
    
    - name: Check Coverage Threshold
      run: |
        mvn jacoco:check
```

### Jenkins Pipeline
```groovy
pipeline {
    agent any
    
    environment {
        JAVA_HOME = tool 'JDK17'
        MAVEN_HOME = tool 'Maven3'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your/repository.git'
            }
        }
        
        stage('Unit Tests') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean test"
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Integration Tests') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn verify -P integration-test"
            }
        }
        
        stage('Coverage') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn jacoco:report"
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'target/site/jacoco',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Report'
                ])
            }
        }
        
        stage('Performance Tests') {
            steps {
                sh "k6 run performance-tests/load-test.js"
            }
        }
    }
}
```

## Casos Reais e Complexos

### 1. Teste de Microserviço com Circuit Breaker
```java
@SpringBootTest
class OrderServiceTest {
    @Autowired
    private OrderService orderService;
    
    @MockBean
    private PaymentServiceClient paymentClient;
    
    @Test
    void shouldHandlePaymentServiceFailure() {
        // Arrange
        OrderRequest request = OrderTestBuilder.aValidOrder().build();
        when(paymentClient.processPayment(any()))
            .thenThrow(new ServiceUnavailableException());
        
        // Act
        Order order = orderService.createOrder(request);
        
        // Assert
        assertEquals(OrderStatus.PENDING_PAYMENT, order.getStatus());
        verify(paymentClient, times(3)).processPayment(any()); // Verifica 3 tentativas
    }
    
    @Test
    @CircuitBreakerTest
    void shouldActivateCircuitBreaker() {
        // Simula falhas múltiplas
        for (int i = 0; i < 5; i++) {
            when(paymentClient.processPayment(any()))
                .thenThrow(new ServiceUnavailableException());
            
            assertThrows(CircuitBreakerException.class, 
                () -> orderService.createOrder(OrderTestBuilder.aValidOrder().build()));
        }
        
        // Verifica se o circuit breaker abriu
        assertTrue(circuitBreaker.isOpen());
    }
}
```

### 2. Teste de Cache Distribuído
```java
@SpringBootTest
class ProductCacheTest {
    @Autowired
    private ProductService productService;
    
    @Autowired
    private RedisCacheManager cacheManager;
    
    @Test
    void shouldUseCacheForProducts() {
        // Primeira chamada - deve ir ao banco
        Product product = productService.getProduct(1L);
        
        // Segunda chamada - deve usar cache
        Product cachedProduct = productService.getProduct(1L);
        
        verify(productRepository, times(1)).findById(1L);
        assertEquals(product, cachedProduct);
        
        // Verifica se está no Redis
        assertTrue(cacheManager.getCache("products").get(1L).isPresent());
    }
    
    @Test
    void shouldInvalidateCacheOnUpdate() {
        // Pré-carrega o cache
        Product product = productService.getProduct(1L);
        
        // Atualiza o produto
        product.setPrice(new BigDecimal("99.99"));
        productService.updateProduct(product);
        
        // Cache deve ser invalidado
        assertFalse(cacheManager.getCache("products").get(1L).isPresent());
    }
}
```

### 3. Teste de Processamento Assíncrono
```java
@SpringBootTest
class AsyncProcessingTest {
    @Autowired
    private OrderProcessor orderProcessor;
    
    @Autowired
    private KafkaTemplate<String, OrderEvent> kafkaTemplate;
    
    @Test
    void shouldProcessOrderAsynchronously() {
        // Arrange
        OrderEvent event = new OrderEvent("123", "CREATE");
        CountDownLatch latch = new CountDownLatch(1);
        AtomicReference<Order> processedOrder = new AtomicReference<>();
        
        // Configura listener
        orderProcessor.setOrderProcessedCallback(order -> {
            processedOrder.set(order);
            latch.countDown();
        });
        
        // Act
        kafkaTemplate.send("orders", event);
        
        // Assert
        assertTrue(latch.await(5, TimeUnit.SECONDS));
        assertNotNull(processedOrder.get());
        assertEquals("123", processedOrder.get().getId());
    }
}
```

## Troubleshooting e Debug de Testes

### 1. Logging em Testes
```java
@Slf4j
@SpringBootTest
class TroubleshootingTest {
    @Rule
    public TestRule watchman = new TestWatcher() {
        @Override
        protected void starting(Description description) {
            log.info("Test starting: {}", description.getMethodName());
        }
        
        @Override
        protected void failed(Throwable e, Description description) {
            log.error("Test failed: {}", description.getMethodName(), e);
        }
    };
    
    @Test
    void shouldLogTestExecution() {
        // Arrange
        log.debug("Setting up test data");
        
        // Act
        log.info("Executing main test logic");
        
        // Assert
        log.debug("Verifying results");
    }
}
```

### 2. Debug de Testes Intermitentes
```java
@SpringBootTest
class FlakynessDebugTest {
    @RepeatedTest(100)
    void shouldHandleConcurrentRequests() {
        ExecutorService executor = Executors.newFixedThreadPool(10);
        CountDownLatch latch = new CountDownLatch(10);
        AtomicInteger failureCount = new AtomicInteger(0);
        
        for (int i = 0; i < 10; i++) {
            executor.submit(() -> {
                try {
                    userService.createUser(UserTestBuilder.aUser().build());
                } catch (Exception e) {
                    failureCount.incrementAndGet();
                    log.error("Failed to create user", e);
                } finally {
                    latch.countDown();
                }
            });
        }
        
        latch.await(10, TimeUnit.SECONDS);
        assertEquals(0, failureCount.get(), 
            "Some concurrent requests failed, check logs for details");
    }
}
```

## Melhores Práticas para Manutenção de Testes

### 1. Organização de Testes
```java
@SpringBootTest
class UserServiceTest {
    // Arrange - Seção de setup
    @Nested
    class Setup {
        @BeforeEach
        void setUp() {
            // Setup comum
        }
        
        @Test
        void shouldInitializeCorrectly() {
            // Testes de inicialização
        }
    }
    
    // Act - Seção de comportamento
    @Nested
    class Behavior {
        @Test
        void shouldCreateUser() {
            // Testes de comportamento
        }
    }
    
    // Assert - Seção de validação
    @Nested
    class Validation {
        @Test
        void shouldValidateUserData() {
            // Testes de validação
        }
    }
}
```

### 2. Documentação de Testes
```java
/**
 * Testes para o serviço de usuários.
 * 
 * Cenários cobertos:
 * - Criação de usuário
 * - Atualização de usuário
 * - Deleção de usuário
 * - Validações de dados
 * 
 * Dependências:
 * - UserRepository
 * - EmailService
 * - SecurityService
 */
@SpringBootTest
class UserServiceDocumentedTest {
    @Test
    @DisplayName("Deve criar usuário com dados válidos")
    @Tag("critical")
    void shouldCreateUserWithValidData() {
        // Test implementation
    }
}
```

### 3. Manutenção de Dados de Teste
```java
@Configuration
class TestDataConfig {
    @Bean
    public DataLoader testDataLoader() {
        return new DataLoader();
    }
}

class DataLoader {
    private static final String TEST_DATA_FILE = "test-data.json";
    
    public TestData loadTestData() {
        try (InputStream is = getClass().getResourceAsStream(TEST_DATA_FILE)) {
            return objectMapper.readValue(is, TestData.class);
        }
    }
}
```

### 4. Limpeza de Recursos
```java
@SpringBootTest
class ResourceCleanupTest {
    private File tempFile;
    private Connection dbConnection;
    
    @BeforeEach
    void setUp() {
        tempFile = Files.createTempFile("test", ".tmp").toFile();
        dbConnection = dataSource.getConnection();
    }
    
    @AfterEach
    void tearDown() {
        if (tempFile != null && tempFile.exists()) {
            tempFile.delete();
        }
        
        if (dbConnection != null) {
            dbConnection.close();
        }
    }
}
```
