# Guia Excel para Analista de Logística Júnior
## PÁGINAS 5-6: FUNÇÕES ESSENCIAIS PARTE 2

### Tabelas Dinâmicas Básicas

#### Estrutura de Dados para Tabela Dinâmica
```
| Data       | Rota | Motorista | Entregas | Status     | Valor     |
|------------|------|-----------|----------|------------|-----------|
| 2024-01-15 | R01  | João      | 12       | Concluído | R$2500,00 |
| 2024-01-15 | R02  | Maria     | 8        | Em Rota   | R$1800,00 |
| 2024-01-15 | R01  | Pedro     | 10       | Concluído | R$2200,00 |
```

#### Configurações Essenciais
1. **Linhas**: Rota, Motorista
2. **Colunas**: Status
3. **Valores**: 
   - Soma de Entregas
   - Soma de Valor
   - Contagem de Status

### Filtros Avançados

#### Critérios de Filtro Comuns
```
| Campo      | Critério 1 | Critério 2 |
|------------|------------|------------|
| Status     | Concluído  | Em Rota    |
| Valor      | >1000      | <5000      |
| Entregas   | >5         |            |
```

#### Aplicações Práticas
1. **Filtros por Data**
   - Entregas do dia
   - Atrasos da semana
   - Performance mensal

2. **Filtros por Performance**
   - Rotas mais eficientes
   - Motoristas mais produtivos
   - Regiões com mais atrasos

### Formatação Condicional

#### Regras Básicas
1. **Alertas de Estoque**
```
| Produto | Quantidade | Status    |
|---------|------------|-----------|
| Item A  | 50        | ⚠️ Baixo   | [Vermelho se <100]
| Item B  | 200       | ✅ Normal  | [Verde se >=100]
```

2. **Monitoramento de Entregas**
```
| Pedido | Prazo    | Status     | Atraso    |
|--------|----------|------------|-----------|
| P001   | 3 dias   | Em Rota    | 🟡 1 dia   |
| P002   | 2 dias   | Atrasado   | 🔴 3 dias  |
```

#### Regras Avançadas
1. **Escalas de Cores**
   - Verde -> Amarelo -> Vermelho para KPIs
   - Gradiente para valores numéricos
   - Ícones para status

2. **Fórmulas Condicionais**
   - Atrasos críticos
   - Estoque crítico
   - Performance abaixo da meta

### Exemplos Práticos Integrados

#### Dashboard de Entregas
```
| Data | Região | Entregas | Meta | Status    | Eficiência |
|------|--------|----------|------|-----------|------------|
| 15/1 | Sul    | 45       | 50   | 90%       | 🟡         |
| 15/1 | Norte  | 55       | 50   | 110%      | 🟢         |
```

#### Análise de Performance
- Tabela Dinâmica mostrando eficiência por região
- Filtros para período específico
- Formatação destacando metas atingidas/não atingidas

### Exercícios Práticos

1. **Tabela Dinâmica**
   Criar análise de:
   - Entregas por motorista/região
   - Valores por status/data
   - Performance por rota/período

2. **Filtros Avançados**
   Desenvolver filtros para:
   - Entregas atrasadas
   - Rotas com baixa eficiência
   - Regiões prioritárias

3. **Formatação Condicional**
   Implementar regras para:
   - Status de estoque
   - Prazos de entrega
   - Metas de performance

### Erros Comuns a Evitar

1. **Tabelas Dinâmicas**
   - Não atualizar dados
   - Ignorar valores nulos
   - Configuração incorreta de campos

2. **Filtros**
   - Critérios conflitantes
   - Ranges incorretos
   - Falta de validação

3. **Formatação**
   - Regras sobrepostas
   - Fórmulas complexas demais
   - Cores confusas

### Dicas Profissionais

1. **Otimização**
   - Use nomes de tabela
   - Crie templates de análise
   - Mantenha backup das configurações

2. **Visualização**
   - Escolha cores consistentes
   - Use ícones claros
   - Mantenha simplicidade

🔍 **Dica Avançada**: Combine tabelas dinâmicas com formatação condicional para criar dashboards interativos e autoatualizáveis.
