# 🔐 Guia Definitivo Spring Security

[![Spring Security](https://img.shields.io/badge/Spring%20Security-6.2.0-brightgreen.svg)](https://spring.io/projects/spring-security)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/your-username/spring-security-guide/issues)

Um guia completo e prático sobre Spring Security, abordando desde conceitos básicos até implementações avançadas para diferentes arquiteturas.

## 📚 Sumário

### [Parte 1: Introdução e Fundamentos](./spring-security-guide-pt1.md)
- Visão geral do Spring Security
- Importância da segurança em aplicações modernas
- Conceitos fundamentais
- Configuração básica
- Dependências necessárias
- Estrutura de classes principais

### [Parte 2: Autenticação e Autorização](./spring-security-guide-pt2.md)
- Modelagem de entidades (User, Role, Permission)
- Scripts SQL e Migrations
- Implementação do UserDetailsService
- Implementação do PasswordEncoder
- Role-Based Access Control (RBAC)
- Method Security
- Expression-based Access Control

### [Parte 3: Implementações por Arquitetura](./spring-security-guide-pt3.md)
- Aplicações MVC
  - Form-based authentication
  - Remember-me functionality
  - Session management
- REST APIs
  - JWT implementation
  - OAuth2 / OpenID Connect
  - Rate limiting
- Microserviços
  - Service-to-service authentication
  - Gateway security
  - Circuit breakers
- IoT e Dispositivos
  - Device authentication
  - Secure MQTT Configuration

### [Parte 4: Boas Práticas e Exemplos](./spring-security-guide-pt4.md)
- Security Headers
- Password Policies
- Audit Logging
- Multi-factor Authentication
- Exemplos práticos completos
- Testes de segurança
- Troubleshooting

## 🚀 Como Usar Este Guia

1. Comece pela Parte 1 se você é iniciante em Spring Security
2. Vá direto para a seção específica se busca uma implementação particular
3. Consulte a Parte 4 para boas práticas e solução de problemas comuns

## 💡 Destaques

- Exemplos de código completos e testáveis
- Diagramas explicativos para fluxos complexos
- Configurações em diferentes formatos (Java, YAML, properties)
- Casos de teste unitários e de integração
- Scripts SQL e migrations
- Integração com diferentes bancos de dados

## 🛠 Pré-requisitos

- Java 17 ou superior
- Spring Boot 3.x
- Maven ou Gradle
- Conhecimento básico de Spring Framework

## 📋 Exemplos de Uso

Cada seção contém exemplos práticos que podem ser executados diretamente. Por exemplo:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .permitAll()
            );
        return http.build();
    }
}
```

## 🤝 Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para:

1. Fork este repositório
2. Criar uma branch para sua feature (`git checkout -b feature/MinhaFeature`)
3. Commit suas mudanças (`git commit -m 'Adicionando nova feature'`)
4. Push para a branch (`git push origin feature/MinhaFeature`)
5. Criar um Pull Request

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## ✨ Agradecimentos

- Comunidade Spring
- Contribuidores do projeto
- Todos que compartilham conhecimento sobre segurança em aplicações

## 📞 Suporte

- Abra uma [issue](https://github.com/your-username/spring-security-guide/issues)
- Entre em contato via [email](mailto:seu-email@exemplo.com)

---

⭐ Se este guia foi útil para você, considere dar uma estrela no repositório!

