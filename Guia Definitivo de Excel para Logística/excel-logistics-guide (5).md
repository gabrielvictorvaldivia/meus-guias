## Dashboards

### KPIs Logísticos Essenciais

#### 1. Painel de Performance Operacional
```excel
// Fórmulas para KPIs principais
Taxa_Entrega = SOMASES(Entregas[Status];"Concluído")/CONT.NÚM(Entregas[ID])
Pontualidade = MÉDIA(SE(Entregas[Atraso]<=0;1;0))
Ocupação_Média = MÉDIA(Cargas[Taxa_Ocupação])
```

Layout Recomendado:
```
+------------------+------------------+
|     OTIF (%)     |  Entregas/Hora  |
|   [Velocímetro]  |    [Medidor]    |
+------------------+------------------+
| Ocupação Veicular|    Fill Rate    |
|    [Gauge]       |   [Percentual]  |
+------------------+------------------+
```

#### 2. Indicadores Financeiros
```excel
// Métricas financeiras chave
Custo_por_Entrega = SOMA(Custos[Valor])/CONT.NÚM(Entregas[ID])
Margem_Operacional = (Receita[Total]-Custos[Total])/Receita[Total]
Break_Even = Custos[Fixo]/(Preço[Médio]-Custos[Variável])
```

### Gráficos Recomendados

#### 1. Análise Temporal
- Line Chart: Tendência de entregas
- Area Chart: Ocupação ao longo do tempo
- Column Chart: Comparativo mensal

```excel
// Configuração de série temporal
Eixo X: Data (agrupado por semana/mês)
Eixo Y primário: Quantidade
Eixo Y secundário: Valor médio
```

#### 2. Análise Geográfica
- Mapa de calor por região
- Gráfico de bolhas por cidade
- Treemap de distribuição

```excel
// Formatação condicional para mapas
Vermelho: Performance < 85%
Amarelo: Performance 85-95%
Verde: Performance > 95%
```

#### 3. Análise de Distribuição
- Histograma: Tempos de entrega
- Box Plot: Outliers de custo
- Pareto: Causas de atrasos

### Layout do Dashboard

#### 1. Estrutura Básica
```
+-----------------+------------------+
|    Filtros      |     Alertas     |
+-----------------+------------------+
|                 |                 |
|    KPIs         |   Tendências    |
|                 |                 |
+-----------------+------------------+
|                                   |
|        Análise Detalhada         |
|                                   |
+-----------------------------------+
```

#### 2. Hierarquia Visual
1. KPIs críticos (topo)
2. Tendências (meio)
3. Detalhamentos (base)
4. Filtros (lateral)

### Atualização Automática

#### 1. Configuração de Refresh
```vba
Sub AtualizarDashboard()
    ' Atualizar conexões
    ThisWorkbook.RefreshAll
    
    ' Recalcular KPIs
    Calculate
    
    ' Atualizar gráficos
    ActiveSheet.ChartObjects.Refresh
End Sub
```

#### 2. Triggers de Atualização
- Ao abrir arquivo
- Timer programado
- Botão manual
- Alteração de filtros

### Elementos Interativos

#### 1. Controles de Filtro
```excel
// Componentes recomendados
- Segmentação de dados (Slicers)
- Timeline para datas
- Caixas de combinação (Combo boxes)
- Botões de opção
```

#### 2. Drill-down
```excel
// Níveis de detalhe
Nível 1: Visão geral (Região)
Nível 2: Estado/Cidade
Nível 3: Centro de Distribuição
Nível 4: Rota específica
```

### Otimização de Performance

#### 1. Boas Práticas
```excel
// Reduzir uso de fórmulas voláteis
EVITAR: AGORA(), HOJE(), INDIRETO()
USAR: Valores calculados em células auxiliares

// Minimizar cálculos
Fórmulas array: Usar com moderação
Referências: Preferir diretas vs. indiretas
```

#### 2. Estrutura de Dados
- Usar tabelas do Excel
- Manter dados próximos
- Evitar referências externas
- Limitar fórmulas condicionais

**⚠️ Notas Importantes:**
- Teste o dashboard com volume real de dados
- Mantenha documentação atualizada
- Implemente controle de versão
- Treine usuários no uso correto

### Recursos Complementares
- Template de Dashboard Logístico
- Biblioteca de Gráficos Personalizados
- Guia de Otimização
- Manual do Usuário

Deseja prosseguir para a próxima seção sobre Automatização?
