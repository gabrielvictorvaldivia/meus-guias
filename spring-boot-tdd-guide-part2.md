## Testes de Segurança

### Configuração de Segurança para Testes
```java
@TestConfiguration
public class SecurityTestConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeRequests()
                .antMatchers("/api/public/**").permitAll()
                .antMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            .and()
            .httpBasic();
        return http.build();
    }
    
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.builder()
            .username("user")
            .password("{bcrypt}$2a$10$GRLdNijSQMUvl/au9ofL.eDwmoohzzS7.rmNSJZ.0FxO/BTk76klW")
            .roles("USER")
            .build();
            
        UserDetails admin = User.builder()
            .username("admin")
            .password("{bcrypt}$2a$10$GRLdNijSQMUvl/au9ofL.eDwmoohzzS7.rmNSJZ.0FxO/BTk76klW")
            .roles("ADMIN")
            .build();
            
        return new InMemoryUserDetailsManager(user, admin);
    }
}
```

### Testes de Autenticação
```java
@WebMvcTest(UserController.class)
@Import(SecurityTestConfig.class)
class SecurityAuthenticationTest {
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void shouldDenyAccessToUnauthenticatedUser() throws Exception {
        mockMvc.perform(get("/api/users"))
            .andExpect(status().isUnauthorized());
    }
    
    @Test
    void shouldAllowAccessToAuthenticatedUser() throws Exception {
        mockMvc.perform(get("/api/users")
            .with(user("user").roles("USER")))
            .andExpect(status().isOk());
    }
    
    @Test
    void shouldAllowAccessWithBasicAuth() throws Exception {
        mockMvc.perform(get("/api/users")
            .with(httpBasic("user", "password")))
            .andExpect(status().isOk());
    }
}
```

### Testes de Autorização com JWT
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class JwtSecurityTest {
    @Autowired
    private JwtTokenProvider tokenProvider;
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Test
    void shouldAccessProtectedEndpointWithValidToken() {
        // Arrange
        String token = tokenProvider.createToken("user", List.of("ROLE_USER"));
        HttpHeaders headers = new HttpHeaders();
        headers.setBearerAuth(token);
        
        // Act
        ResponseEntity<String> response = restTemplate.exchange(
            "/api/protected",
            HttpMethod.GET,
            new HttpEntity<>(headers),
            String.class
        );
        
        // Assert
        assertEquals(HttpStatus.OK, response.getStatusCode());
    }
    
    @Test
    void shouldRejectExpiredToken() {
        // Arrange
        String expiredToken = "eyJhbGciOiJIUzI1NiJ9..."; // Token expirado
        HttpHeaders headers = new HttpHeaders();
        headers.setBearerAuth(expiredToken);
        
        // Act & Assert
        ResponseEntity<String> response = restTemplate.exchange(
            "/api/protected",
            HttpMethod.GET,
            new HttpEntity<>(headers),
            String.class
        );
        
        assertEquals(HttpStatus.UNAUTHORIZED, response.getStatusCode());
    }
}
```

### Testes de Autorização por Role
```java
@SpringBootTest
@Import(SecurityTestConfig.class)
class RoleBasedAuthorizationTest {
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void shouldAllowAdminAccess() throws Exception {
        mockMvc.perform(get("/api/admin")
            .with(user("admin").roles("ADMIN")))
            .andExpect(status().isOk());
    }
    
    @Test
    void shouldDenyUserAccessToAdminEndpoint() throws Exception {
        mockMvc.perform(get("/api/admin")
            .with(user("user").roles("USER")))
            .andExpect(status().isForbidden());
    }
}
```

## Cobertura de Testes

### Configuração do JaCoCo
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.8</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
        <execution>
            <id>check</id>
            <goals>
                <goal>check</goal>
            </goals>
            <configuration>
                <rules>
                    <rule>
                        <element>PACKAGE</element>
                        <limits>
                            <limit>
                                <counter>LINE</counter>
                                <value>COVEREDRATIO</value>
                                <minimum>0.80</minimum>
                            </limit>
                        </limits>
                    </rule>
                </rules>
            </configuration>
        </execution>
    </executions>
</plugin>
```

### Métricas de Cobertura

#### Tipos de Métricas
1. **Line Coverage**: Percentual de linhas de código executadas
2. **Branch Coverage**: Percentual de ramificações (if/else) executadas
3. **Method Coverage**: Percentual de métodos executados
4. **Class Coverage**: Percentual de classes testadas

#### Exemplo de Relatório
```
-------------------------------------------------------------------------------
Package          Class Coverage | Method Coverage | Line Coverage | Branch Coverage
-------------------------------------------------------------------------------
controllers         95%             90%              88%             85%
services            92%             88%              87%             82%
repositories        88%             85%              83%             80%
domain             100%             95%              93%             90%
-------------------------------------------------------------------------------
```

### Estratégias para Aumentar Cobertura

1. **Identificação de Pontos Críticos**
```java
@Test
void shouldHandleEdgeCases() {
    // Teste para valor nulo
    assertThrows(IllegalArgumentException.class, () -> service.process(null));
    
    // Teste para lista vazia
    assertTrue(service.process(Collections.emptyList()).isEmpty());
    
    // Teste para valor limite
    assertEquals(MAX_VALUE, service.process(Arrays.asList(1, 2, 3)));
}
```

2. **Testes Paramétricos**
```java
@ParameterizedTest
@CsvSource({
    "1, 1, 2",
    "0, 5, 5",
    "-1, 1, 0",
    "10, -5, 5"
})
void shouldCalculateWithVariousInputs(int a, int b, int expected) {
    assertEquals(expected, calculator.add(a, b));
}
```

3. **Testes de Exception**
```java
@Test
void shouldHandleExceptions() {
    // Arrange
    String invalidInput = "invalid";
    
    // Act & Assert
    BusinessException exception = assertThrows(
        BusinessException.class,
        () -> service.process(invalidInput)
    );
    
    assertEquals("Invalid input provided", exception.getMessage());
}
```

### Boas Práticas de Cobertura

1. **Definir Metas Realistas**
- 80% de cobertura geral como objetivo mínimo
- 90% para código crítico de negócio
- 100% para utilitários e validadores

2. **Priorização de Testes**
- Focar em código com maior complexidade ciclomática
- Testar casos de borda e exceções
- Cobrir fluxos críticos de negócio

3. **Manutenção Contínua**
- Executar relatórios de cobertura regularmente
- Integrar verificação de cobertura no CI/CD
- Revisar e atualizar testes existentes
