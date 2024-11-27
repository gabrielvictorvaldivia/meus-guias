## Análise de Dados Logísticos

### 1. Tempo de Entrega (Lead Time)

#### Fórmula de Cálculo
```excel
// Lead Time básico
=[@Data_Entrega]-[@Data_Pedido]

// Lead Time útil (excluindo fins de semana)
=DIAÚTIL([@Data_Pedido];[@Data_Entrega])

// OTIF (On Time In Full)
=SE(E([@Data_Entrega]<=[@Data_Prometida];
      [@Qtd_Entregue]=[@Qtd_Pedida]);1;0)
```

#### Visualização Recomendada
- Gráfico de linhas para tendência temporal
- Histograma para distribuição de tempos
- Box plot para identificar outliers

#### Interpretação e Ações
| Indicador | Bom | Regular | Crítico | Ação Corretiva |
|-----------|-----|----------|----------|----------------|
| Lead Time | <48h | 48-72h | >72h | Revisar rotas/processos |
| OTIF | >95% | 90-95% | <90% | Análise de causa raiz |

### 2. Taxa de Ocupação de Veículos

#### Fórmulas Principais
```excel
// Ocupação volumétrica
=[@Volume_Carregado]/[@Capacidade_Volume]

// Ocupação por peso
=[@Peso_Carregado]/[@Capacidade_Peso]

// Ocupação efetiva (considera ambos)
=MÁXIMO([@Ocupação_Volume];[@Ocupação_Peso])
```

#### Métricas de Análise
- Taxa média de ocupação
- Variação por rota/período
- Custo por m³ transportado

### 3. Custos de Transporte

#### Cálculos Essenciais
```excel
// Custo por km
=[@Custo_Total]/[@Distância]

// Custo por entrega
=[@Custo_Total]/[@Quantidade_Entregas]

// Break-even point
=SOMASES(Custos[Fixo])/
 (MÉDIA(Fretes[Valor])-MÉDIA(Custos[Variável]))
```

#### Dashboard de Custos
- Custos fixos vs. variáveis
- Tendência mensal
- Comparativo por modal
- Análise de rentabilidade por rota

### 4. Performance de Rotas

#### Indicadores Chave
```excel
// Eficiência de rota
=[@Distância_Planejada]/[@Distância_Real]

// Entregas por hora
=[@Quantidade_Entregas]/[@Tempo_Total_Horas]

// Desvio de tempo
=([@Tempo_Real]-[@Tempo_Previsto])/[@Tempo_Previsto]
```

#### Análise Geográfica
- Mapeamento de pontos críticos
- Densidade de entregas por região
- Tempo médio por zona

### 5. Nível de Serviço

#### Métricas Principais
```excel
// Fill rate
=SOMASES(Pedidos[Qtd_Entregue])/
 SOMA(Pedidos[Qtd_Solicitada])

// Perfect Order Rate
=SE(E([@No_Prazo]=1;
      [@Sem_Avaria]=1;
      [@Documentação_OK]=1);1;0)

// Customer Satisfaction Score
=MÉDIA(Avaliações[Nota])
```

### 6. Acuracidade de Inventário

#### Fórmulas de Controle
```excel
// Acuracidade por item
=SE([@Estoque_Físico]=[@Estoque_Sistema];1;0)

// Acuracidade geral
=CONTAR.SE(Inventário[Status];"OK")/
 CONTAR.NÚMS(Inventário[Status])

// Divergência valorizada
=SOMA(ABS([@Qtd_Física]-[@Qtd_Sistema])*[@Custo_Unit])
```

### Dicas de Profissionais

> "Monitore seus KPIs diariamente, mas analise tendências semanalmente. Isso evita reações exageradas a variações pontuais."
> - Roberto Almeida, Diretor de Logística

> "Mantenha um histórico rolante de 13 meses para comparações anuais precisas."
> - Lucia Fernandes, Analista de Performance

### Armadilhas Comuns a Evitar
1. Não considerar sazonalidades na análise
2. Ignorar outliers sem investigação
3. Misturar diferentes tipos de operação
4. Não segmentar análises por cliente/região

### Automatização Recomendada
- Criar rotina diária de atualização
- Implementar alertas para desvios
- Gerar relatórios automáticos
- Manter dashboard live

**⚠️ Nota Importante:**
Sempre valide os dados antes de tomar decisões críticas. Estabeleça um processo de verificação regular da qualidade dos dados.

### Recursos Complementares
- Template de Dashboard Logístico
- Biblioteca de KPIs
- Guia de Análise de Causa Raiz
- Calendário de Medição e Reporting

Deseja prosseguir para a próxima seção sobre Tabelas Dinâmicas para Logística?
