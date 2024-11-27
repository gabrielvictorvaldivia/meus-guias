# Guia Definitivo de Excel em Logística

## Índice
1. [Estrutura Inicial](#estrutura-inicial)
2. [Organização de Dados](#organizacao-de-dados)
3. [Fórmulas Fundamentais](#formulas-fundamentais)
4. [Análise de Dados Logísticos](#analise-de-dados)
5. [Tabelas Dinâmicas](#tabelas-dinamicas)
6. [Dashboards](#dashboards)
7. [Automatização](#automatizacao)
8. [Resolução de Problemas](#resolucao-de-problemas)

## Estrutura Inicial

### Introdução
A análise de dados tornou-se o coração da logística moderna. Em um mercado onde a eficiência operacional pode significar a diferença entre lucro e prejuízo, dominar ferramentas de análise como o Microsoft Excel é fundamental para profissionais de logística. Este guia fornecerá as ferramentas necessárias para transformar dados brutos em insights acionáveis.

### Pré-requisitos Técnicos
- Microsoft Excel 2016 ou superior (recomendado Excel 365)
- Conhecimentos básicos de operações em Excel
- Computador com mínimo de 8GB RAM para melhor performance

### Como Usar este Guia
1. Siga as seções sequencialmente
2. Complete os exercícios práticos
3. Consulte o glossário quando necessário
4. Salve os modelos fornecidos como referência

### Glossário de Termos Logísticos
- **Lead Time**: Tempo total desde o pedido até a entrega
- **OTIF** (On Time In Full): Entrega no prazo e completa
- **Fill Rate**: Taxa de atendimento de pedidos
- **Stock Turn**: Giro de estoque
- **Cross-docking**: Distribuição sem armazenamento
- **Last Mile**: Última etapa da entrega
- **WMS**: Warehouse Management System
- **TMS**: Transportation Management System
- **KPI**: Key Performance Indicator
- **SKU**: Stock Keeping Unit

## Organização de Dados

### Estruturação de Dados
A base de uma análise eficiente está na organização adequada dos dados. Siga estas diretrizes:

#### Melhores Práticas
1. Uma linha por transação/evento
2. Colunas consistentes e bem definidas
3. Sem células mescladas
4. Sem espaços em branco desnecessários
5. Dados brutos separados de análises

#### Convenções de Nomenclatura

**Para Colunas:**
- ID_[tipo]: Identificadores únicos
- DT_[evento]: Campos de data
- VL_[metrica]: Valores monetários
- QT_[item]: Quantidades
- FL_[status]: Flags/indicadores

**Para Abas:**
- RAW_: Dados brutos
- ANA_: Análises
- DAS_: Dashboards
- AUX_: Tabelas auxiliares
- CFG_: Configurações

### Formatação Condicional

**Regras Essenciais:**
```excel
// Atrasos em entregas
=SE([@[Data Real]]-[@[Data Prevista]]>0;"VERMELHO";"VERDE")

// Nível de Estoque
=SE([@Estoque]<[@[Estoque Mínimo]];"VERMELHO";
  SE([@Estoque]>[@[Estoque Máximo]];"AMARELO";"VERDE"))
```

### Validação de Dados

**Exemplos Práticos:**
```excel
// Lista de Status de Entrega
=AUX_Status[Status_Entrega]

// Datas Válidas
=E([@Data]>=HOJE()-365;[@Data]<=HOJE())

// Valores Numéricos
=E([@Quantidade]>0;[@Quantidade]<=1000)
```

**Dica Profissional:** "Sempre crie uma aba de parametrização (AUX_) para centralizar todas as listas de validação. Isso facilita a manutenção e garante consistência." - Ana Silva, Coordenadora de Logística

**⚠️ Nota de Aviso:**
Nunca delete as abas de dados brutos (RAW_). Sempre mantenha um backup antes de aplicar transformações importantes.

Deseja prosseguir para a próxima seção sobre Fórmulas Fundamentais?
