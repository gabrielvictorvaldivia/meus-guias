# Visão Geral do Projeto .NET Web API

Este documento fornece um overview detalhado da estrutura de arquivos e da configuração inicial de uma API criada com o comando `dotnet new webapi`. A compreensão desses arquivos é fundamental para o desenvolvimento, configuração e depuração da aplicação.

## Estrutura de Arquivos

A estrutura inicial gerada pelo .NET é limpa e funcional, fornecendo o essencial para iniciar o desenvolvimento:

```
.
├── Program.cs
├── Properties
│   └── launchSettings.json
├── appsettings.Development.json
├── appsettings.json
└── obj/
```

-----

## Análise dos Arquivos

### `Program.cs`

Este é o **coração da sua aplicação**. A partir do .NET 6, com a introdução das "Minimal APIs", este arquivo unifica a inicialização, configuração e definição de rotas, tornando o código de entrada muito mais conciso.

**Principais Responsabilidades:**

  * **Criação do Host:** Inicializa o host da aplicação web (`WebApplication.CreateBuilder(args)`), que é o processo responsável por escutar as requisições HTTP.
  * **Configuração de Serviços (Injeção de Dependência):** É aqui que você registra os serviços que a aplicação utilizará. O acesso a esses serviços é feito através de `builder.Services`. Exemplos comuns incluem:
      * `builder.Services.AddDbContext<MyDbContext>();` (Para Entity Framework Core)
      * `builder.Services.AddAuthentication();` (Para configurar autenticação)
      * `builder.Services.AddSwaggerGen();` (Para gerar a documentação da API)
  * **Definição do Pipeline de Requisições HTTP:** Configura os *middlewares* através do objeto `app`. Middlewares são componentes que processam a requisição e a resposta em sequência. **A ordem de registro é crucial.** Exemplos:
      * `app.UseHttpsRedirection();`
      * `app.UseAuthentication();`
      * `app.UseAuthorization();`
  * **Mapeamento de Endpoints:** Define as rotas (endpoints) da sua API, associando um verbo HTTP (GET, POST, PUT, DELETE) e uma rota (ex: `/users`) a uma função que será executada.
      * `app.MapGet("/weatherforecast", () => { ... });`

### `Properties/launchSettings.json`

Este arquivo contém configurações utilizadas **apenas para o ambiente de desenvolvimento local**. Ele não é publicado com a aplicação e não tem efeito em ambientes de produção.

**O que ele define?**

  * **Perfis de Execução (`profiles`):** Permite criar diferentes maneiras de iniciar a aplicação localmente (via `dotnet run` ou Visual Studio). Comumente, você encontrará perfis para `http` e `https`.
  * **Variáveis de Ambiente:** Sua função mais importante é definir a variável de ambiente `ASPNETCORE_ENVIRONMENT` como `"Development"`. Isso sinaliza para a aplicação que ela deve ativar configurações específicas de desenvolvimento.
  * **URLs e Portas (`applicationUrl`):** Especifica em quais portas HTTP e HTTPS a aplicação será executada localmente (ex: `https://localhost:7123`).
  * **Comportamento de Lançamento:** Controla se um navegador deve ser aberto (`launchBrowser`) e qual URL inicial deve ser acessada (`launchUrl`, frequentemente definido como `swagger`).

<!-- end list -->

```json
{
  "profiles": {
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "launchUrl": "swagger",
      "applicationUrl": "https://localhost:7123;http://localhost:5123",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

### `appsettings.json` e `appsettings.Development.json`

Estes arquivos gerenciam as configurações da sua aplicação de forma hierárquica.

  * **`appsettings.json`**: Contém as configurações **base**, padrão ou de produção. São valores que servem como fallback ou que são comuns a todos os ambientes.

    ```json
    {
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft.AspNetCore": "Warning"
        }
      },
      "AllowedHosts": "*"
    }
    ```

  * **`appsettings.Development.json`**: Contém configurações que **sobrescrevem** as do `appsettings.json` quando a aplicação está rodando no ambiente de `Development`. Isso permite, por exemplo, usar uma connection string de um banco de dados local para desenvolvimento sem alterar a connection string de produção.

> **Como funciona?** O .NET primeiro carrega `appsettings.json` e, em seguida, carrega `appsettings.{Environment}.json`. Se uma mesma chave de configuração existir em ambos, o valor do arquivo mais específico (o de ambiente) prevalecerá.

### `obj/` (Diretório de Objetos)

Este diretório contém arquivos intermediários e caches gerados pelo processo de build do .NET.

> **Importante:** Este diretório **NÃO** deve ser incluído no seu sistema de controle de versão (como o Git). Ele deve ser listado no seu arquivo `.gitignore`. Se for apagado, será recriado automaticamente no próximo build.

  * **`project.assets.json`**: Um arquivo essencial para o NuGet (gerenciador de pacotes). Ele mapeia o grafo completo de todas as dependências do projeto, incluindo as dependências das dependências (transitivas). Garante que a restauração de pacotes (`dotnet restore`) seja rápida e consistente.
  * **`project.nuget.cache`**: Um arquivo de cache usado pelo NuGet para acelerar o processo de build, armazenando resultados de resoluções de pacotes.
