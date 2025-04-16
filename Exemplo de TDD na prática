# TDD na Prática: Classe Cliente em Java

Este guia mostra o processo passo a passo do desenvolvimento orientado a testes (TDD) para uma classe `Cliente`, utilizando **Java** e o framework **JUnit**.

---

## ✨ Visão Geral do TDD

> **TDD = Red → Green → Refactor**
>
> 1. **Red:** Escreve um teste que falha.
> 2. **Green:** Implementa o mínimo para o teste passar.
> 3. **Refactor:** Refatora o código mantendo os testes verdes.

---

## 🔧 1. Preparando o Ambiente

**Requisitos:**
- JDK 17+
- JUnit 5
- Um projeto com estrutura semelhante a:

```
/src/main/java/com/exemplo/Cliente.java
/src/test/java/com/exemplo/ClienteTest.java
```

Se estiver usando Maven, adicione no `pom.xml`:

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

---

## 🔴 2. Primeiro Teste: Criar Cliente (Red)

Arquivo: `ClienteTest.java`

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class ClienteTest {

    @Test
    public void deveCriarClienteComNomeEmail() {
        Cliente cliente = new Cliente("Gabriel", "gabriel@email.com");

        assertEquals("Gabriel", cliente.getNome());
        assertEquals("gabriel@email.com", cliente.getEmail());
    }
}
```

> Esse teste vai falhar inicialmente, porque a classe `Cliente` ainda não existe.

---

## 🔵 3. Implementa o Mínimo (Green)

Arquivo: `Cliente.java`

```java
public class Cliente {
    private String nome;
    private String email;

    public Cliente(String nome, String email) {
        this.nome = nome;
        this.email = email;
    }

    public String getNome() {
        return nome;
    }

    public String getEmail() {
        return email;
    }
}
```

Rode os testes. Se tudo passar, você está no **Green**.

---

## ⚒️ 4. Refatorar (Refactor)

Aqui ainda não há necessidade de refatorar, mas já podemos pensar em futuras melhorias: tornar a classe `record` se for imutável, validar dados com objetos de valor, etc.

---

## 📄 5. Validar E-mail

### Teste:

```java
@Test
public void naoDevePermitirEmailInvalido() {
    assertThrows(IllegalArgumentException.class, () -> {
        new Cliente("Gabriel", "email_invalido");
    });
}
```

### Implementa validação:

```java
public Cliente(String nome, String email) {
    if (!email.matches("^[^@\s]+@[^@\s]+\\.[^@\s]+$")) {
        throw new IllegalArgumentException("Email inválido");
    }
    this.nome = nome;
    this.email = email;
}
```

---

## 📅 6. Nome Obrigatório

### Teste:

```java
@Test
public void naoDevePermitirNomeVazio() {
    assertThrows(IllegalArgumentException.class, () -> {
        new Cliente("", "gabriel@email.com");
    });
}
```

### Implementa:

```java
if (nome == null || nome.trim().isEmpty()) {
    throw new IllegalArgumentException("Nome obrigatório");
}
```

---

## 🧰 Considerações Finais

### Testes Criados:
- `deveCriarClienteComNomeEmail()`
- `naoDevePermitirEmailInvalido()`
- `naoDevePermitirNomeVazio()`

### Benefícios do TDD:
- Criação guiada pelas regras de negócio.
- Feedback rápido.
- Código mais confiável.
- Melhor design emergente.

---

Se quiser evoluir, próximos passos:
- Validar CPF
- Adicionar persistência fake ou mockada
- Criar serviços que manipulam clientes com testes

Pronto para subir esse projeto como base para um módulo de clientes real.

---

