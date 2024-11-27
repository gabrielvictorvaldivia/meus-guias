## Tabelas Dinâmicas para Logística

### Criação de Tabelas Dinâmicas Eficientes

#### Estrutura Base Recomendada
```excel
// Configuração típica para análise de entregas
Linhas: Data, Região, Cidade
Colunas: Status de Entrega
Valores: Quantidade de Entregas, Valor Total
Filtros: Cliente, Transportadora
```

#### Boas Práticas
1. Atualizar a origem dos dados antes de cada análise
2. Usar tabelas como fonte de dados
3. Evitar referências quebradas
4. Manter formatação consistente

### Agrupamentos Úteis para Logística

#### Por Período
```excel
// Configurações recomendadas
Anos > Trimestres > Meses > Semanas > Dias
Agrupar por:
- Dias úteis
- Semanas operacionais
- Períodos de pico
```

#### Por Geografia
```excel
// Hierarquia geográfica
Região > Estado > Cidade > Bairro
Adicionar:
- Zonas de entrega
- Áreas de cobertura
- Filiais responsáveis
```

### Campos Calculados Importantes

#### Métricas de Performance
```excel
// Taxa de Entrega no Prazo
=QUANTIDADE/TOTAL_ENTREGAS

// Custo por Entrega
=SOMA_CUSTOS/QUANTIDADE

// Densidade de Entregas
=QUANTIDADE/ÁREA_COBERTURA
```

#### Análises Temporais
```excel
// Variação Período Anterior
=([Atual]-[Anterior])/[Anterior]

// Média Móvel
=MÉDIA(DESLOCAMENTO(-3):ATUAL)
```

### Relatórios Personalizados

#### 1. Relatório de Performance Diária
```excel
Linhas:
- Data
- Transportadora
- Rota

Valores:
- Quantidade Prevista
- Quantidade Realizada
- % Atingimento
- Custo Médio
```

#### 2. Análise de Custos
```excel
Linhas:
- Tipo de Operação
- Modal
- Região

Valores:
- Custo Total
- Custo/Kg
- Custo/m³
- Margem Operacional
```

#### 3. Monitoramento de SLA
```excel
Linhas:
- Cliente
- Serviço
- Prazo

Valores:
- OTIF
- Fill Rate
- Lead Time Médio
- Desvio Padrão
```

### Dicas Avançadas

#### 1. Segmentação de Dados
- Usar segmentadores conectados
- Criar linha do tempo interativa
- Vincular múltiplas tabelas
- Atualização sincronizada

#### 2. Formatação Condicional
```excel
// Regras sugeridas
Vermelho: < Meta - 10%
Amarelo: Entre Meta -10% e Meta
Verde: > Meta
```

#### 3. Campos Calculados Avançados
```excel
// Produtividade
=[Entregas Realizadas]/[Horas Trabalhadas]

// Custo Efetivo
=[Custo Total]/[Entregas Bem Sucedidas]

// Índice de Eficiência
=[OTIF]*[Ocupação Veicular]*[Margem]
```

### Resolução de Problemas Comuns

#### 1. Performance
- Limitar número de campos calculados
- Usar configuração "Adiar Layout"
- Evitar fórmulas complexas
- Otimizar origem dos dados

#### 2. Precisão
- Verificar duplicidades
- Validar totalizadores
- Conferir agrupamentos
- Testar filtros

### Melhores Práticas para Atualização

1. **Processo de Refresh**
```excel
- Atualizar fonte de dados
- Verificar conexões
- Limpar cache se necessário
- Validar totais
```

2. **Automatização**
```excel
// Macro básica de atualização
Sub AtualizarTD()
    ActiveSheet.PivotTables("TD_Entregas").RefreshTable
    ActiveSheet.PivotTables("TD_Custos").RefreshTable
End Sub
```

**⚠️ Notas Importantes:**
- Sempre documente as alterações em tabelas dinâmicas
- Mantenha um backup antes de modificações significativas
- Valide os números com outras fontes
- Estabeleça um processo de revisão periódica

### Recursos Adicionais
- Templates de Tabelas Dinâmicas
- Biblioteca de Campos Calculados
- Guia de Troubleshooting
- Modelos de Relatórios

Deseja prosseguir para a próxima seção sobre Dashboards?
