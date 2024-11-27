# Guia Excel para Analista de Logística Júnior
## PÁGINAS 7-8: ANÁLISE DE DADOS LOGÍSTICOS

### Controle de Estoque

#### Modelo de Controle Básico
```
| SKU    | Produto | Qtd Atual | Mín | Máx | Giro | Lead Time |
|--------|---------|-----------|-----|-----|------|-----------|
| P001   | Caixa P | 150       | 100 | 300 | 50   | 5 dias    |
| P002   | Caixa M | 80        | 100 | 250 | 30   | 7 dias    |
| P003   | Caixa G | 200       | 150 | 400 | 40   | 10 dias   |
```

#### Fórmulas de Gestão de Estoque
1. **Ponto de Ressuprimento**
```
=SOMA(Giro_Diário * Lead_Time, Estoque_Segurança)
```

2. **Cobertura de Estoque**
```
=QUOCIENTE(Qtd_Atual, Média_Consumo_Diário)
```

### Análise de Rotas

#### Tabela de Roteirização
```
| Rota | Origem | Destino | Km    | Entregas | Tempo Médio |
|------|---------|---------|-------|----------|-------------|
| R01  | CD Sul  | Zona 1  | 45    | 12       | 4h         |
| R02  | CD Sul  | Zona 2  | 38    | 8        | 3h         |
| R03  | CD Norte| Zona 3  | 52    | 15       | 5h         |
```

#### KPIs de Rota
1. **Eficiência por Km**
```
=DIVISÃO(Total_Entregas, Total_Km)
```

2. **Custo por Entrega**
```
=SOMA(Custo_Fixo, (Custo_Km * Total_Km)) / Total_Entregas
```

### Acompanhamento de Entregas

#### Painel de Monitoramento
```
| Data | Pedido | Cliente | SLA  | Realizado | Status    | Desvio |
|------|---------|---------|------|-----------|-----------|--------|
| 15/1 | P001    | ABC Ltd | 24h  | 22h       | ✅ No Prazo| -2h    |
| 15/1 | P002    | XYZ Inc | 48h  | 52h       | ⚠️ Atrasado| +4h    |
```

#### Cálculos de Performance
1. **On-Time Delivery (OTD)**
```
=CONT.SES(Status;"No Prazo") / CONT.NÚM(Status)
```

2. **Tempo Médio de Entrega**
```
=MÉDIA(Tempo_Realizado)
```

### KPIs Básicos

#### Painel de Indicadores
```
| Indicador          | Meta  | Atual | Status    |
|-------------------|-------|-------|-----------|
| OTD (%)           | 95    | 92    | 🟡        |
| Fill Rate (%)     | 98    | 97    | 🟢        |
| Custo/Entrega    | 25    | 28    | 🔴        |
```

#### Fórmulas de Cálculo
1. **Fill Rate**
```
=SOMASE(Status;"Completo") / CONT.NÚM(Pedidos)
```

2. **OTIF (On Time In Full)**
```
=(Entregas_No_Prazo_E_Completas / Total_Entregas) * 100
```

### Exercícios Práticos

1. **Gestão de Estoque**
   - Calcule o ponto de ressuprimento
   - Analise a cobertura de estoque
   - Identifique itens críticos

2. **Análise de Rotas**
   - Calcule eficiência por rota
   - Determine custo por entrega
   - Otimize roteirização

3. **Performance de Entregas**
   - Calcule OTD e OTIF
   - Analise tempos de entrega
   - Identifique gargalos

### Erros Comuns a Evitar

1. **Controle de Estoque**
   - Ignorar sazonalidade
   - Não considerar lead time
   - Esquecer estoque em trânsito

2. **Análise de Rotas**
   - Desconsiderar horários de pico
   - Ignorar restrições de entrega
   - Não atualizar quilometragens

3. **Monitoramento**
   - Dados desatualizados
   - Falta de padronização
   - Indicadores mal calculados

### Dicas Profissionais

1. **Gestão Eficiente**
   - Mantenha histórico de dados
   - Use validações cruzadas
   - Automatize cálculos repetitivos

2. **Análise Inteligente**
   - Compare períodos similares
   - Considere fatores externos
   - Documente anomalias

🔍 **Dica Avançada**: Crie um dashboard integrado combinando todos os KPIs principais para uma visão gerencial completa.
