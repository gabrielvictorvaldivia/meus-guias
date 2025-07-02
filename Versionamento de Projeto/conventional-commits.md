# Guia de Padrão de Commits (Conventional Commits)

Este guia define o padrão de mensagens de commit a ser utilizado neste projeto. Adotar um padrão consistente nos ajuda a:

  - **Melhorar a legibilidade:** Entender rapidamente o que cada alteração faz.
  - **Automatizar o versionamento:** Gerar `CHANGELOGs` automaticamente.
  - **Facilitar a colaboração:** Manter um histórico de commits limpo e organizado para todos os contribuidores.

Baseamo-nos na especificação [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

## Estrutura do Commit

Cada mensagem de commit deve seguir a seguinte estrutura:

```
<tipo>[escopo opcional]: <descrição>

[corpo opcional]

[rodapé opcional]
```

-----

### 1\. Tipo (`<tipo>`)

O tipo deve ser um dos seguintes, indicando a natureza da alteração.

  * **`feat`**: Para novas funcionalidades (`feature`).

    > *Exemplo: Adição de um novo endpoint, uma nova validação na model.*

  * **`fix`**: Para correções de bugs (`bug fix`).

    > *Exemplo: Correção de um cálculo errado, um null reference exception.*

  * **`build`**: Para alterações que afetam o sistema de build ou dependências externas.

    > *Exemplo: Modificar o arquivo `.csproj`, atualizar versões de pacotes NuGet, configurar o Dockerfile.*

  * **`ci`**: Para alterações em arquivos e scripts de Integração Contínua (CI).

    > *Exemplo: Configurar workflows do GitHub Actions ou pipelines do Azure DevOps.*

  * **`docs`**: Para alterações exclusivamente na documentação.

    > *Exemplo: Adicionar comentários no código, atualizar este `README.md`.*

  * **`perf`**: Para uma alteração de código que melhora o desempenho.

    > *Exemplo: Otimizar uma query no banco de dados, reduzir o uso de memória em um algoritmo.*

  * **`refactor`**: Para uma alteração de código que não corrige um bug nem adiciona uma funcionalidade, mas melhora a estrutura ou legibilidade.

    > *Exemplo: Renomear variáveis, extrair um método, aplicar um design pattern.*

  * **`style`**: Para alterações que não afetam o significado do código.

    > *Exemplo: Formatação, espaços em branco, ponto e vírgula ausente, regras de lint.*

  * **`test`**: Para adicionar ou corrigir testes existentes.

    > *Exemplo: Criar testes de unidade para um serviço, configurar testes de integração.*

  * **`chore`**: Para outras alterações que não modificam o código-fonte ou os testes.

    > *Exemplo: Atualizar o `.gitignore`, adicionar ou modificar arquivos de configuração que não são de build.*

### 2\. Escopo (`[escopo opcional]`)

O escopo é opcional e serve para fornecer um contexto adicional sobre a parte do código que foi alterada. Para um projeto .NET, alguns exemplos de escopo poderiam ser:

  * `auth` (Autenticação/Autorização)
  * `user-controller`
  * `product-service`
  * `database`
  * `infra`
  * `validation`

### 3\. Descrição (`<descrição>`)

Uma descrição curta e concisa da alteração.

  * Use o tempo verbal **imperativo** (ex: "adiciona", "corrige", "altera" ao invés de "adicionado", "corrigido", "alterado").
  * Comece com letra minúscula.
  * Não termine com um ponto final (`.`).

### 4\. Corpo (`[corpo opcional]`)

O corpo é opcional e usado para fornecer mais detalhes sobre a alteração, explicando o "porquê" da mudança, em vez do "como".

### 5\. Rodapé (`[rodapé opcional]`)

O rodapé é opcional e usado para referenciar issues ou para indicar **Breaking Changes**.

  * **Breaking Change**: Uma alteração que quebra a compatibilidade com versões anteriores. Deve começar com `BREAKING CHANGE:` seguido por uma descrição da quebra.

-----

## Exemplos Práticos para um Projeto .NET

#### Commit simples (feat)

```
feat: adiciona endpoint para criação de pedidos
```

#### Commit com escopo (fix)

```
fix(auth): corrige lógica de expiração do token JWT
```

#### Commit com corpo (refactor)

```
refactor(ProductService): extrai lógica de cálculo de imposto para classe dedicada

A classe ProductService estava acumulando muitas responsabilidades.
A lógica de cálculo de impostos foi movida para TaxCalculatorService
para seguir o Princípio da Responsabilidade Única (SRP).
```

#### Commit para atualização de dependência (build)

```
build: atualiza Microsoft.EntityFrameworkCore para v8.0.6
```

#### Commit para adicionar testes (test)

```
test(UserController): adiciona testes de unidade para o método GetById
```

#### Commit com Breaking Change (feat)

```
feat(user): altera o tipo da propriedade Id de int para Guid

BREAKING CHANGE: O identificador de usuário (Id) foi alterado de
um `int` para `Guid`. Todas as tabelas relacionadas e DTOs que
utilizam o Id do usuário precisam ser atualizadas para refletir
essa mudança no tipo de dado.
```
