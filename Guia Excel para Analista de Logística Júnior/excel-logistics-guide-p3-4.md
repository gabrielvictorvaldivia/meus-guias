# Guia Excel para Analista de Logística Júnior
## PÁGINAS 3-4: FUNÇÕES ESSENCIAIS PARTE 1

### PROCV para Busca de Dados

#### Estrutura Básica
```
=PROCV(valor_procurado; matriz_tabela; núm_coluna; FALSO)
```

#### Exemplo Prático - Tabela de Produtos
```
| Código | Produto      | Preço R$ | Estoque | Local      |
|--------|-------------|----------|---------|------------|
| P001   | Caixa 20L   | 25,00    | 150     | Armazém A |
| P002   | Caixa 50L   | 45,00    | 200     | Armazém B |
| P003   | Caixa 100L  | 80,00    | 75      | Armazém A |
```

#### Aplicação em Logística
```
| Pedido | Código | Quantidade | [Produto]   | [Local]    |
|--------|--------|------------|-------------|------------|
| 001    | P001   | 10         | =PROCV...   | =PROCV...  |
```

### SOMASE para Totalizações

#### Estrutura Básica
```
=SOMASE(intervalo_critério; critério; intervalo_soma)
```

#### Exemplo Prático - Controle de Estoque
```
| Local      | Produto    | Quantidade | Valor Unit |
|------------|------------|------------|------------|
| Armazém A  | Caixa 20L  | 100        | 25,00      |
| Armazém B  | Caixa 20L  | 150        | 25,00      |
| Armazém A  | Caixa 50L  | 75         | 45,00      |
```

#### Fórmulas Práticas
1. Total por Armazém:
```
=SOMASE(A2:A10;"Armazém A";C2:C10)
```

2. Total por Produto:
```
=SOMASE(B2:B10;"Caixa 20L";C2:C10)
```

### Exemplos Combinados

#### Controle de Expedição
```
| Data       | Rota | Entregas | Valor Total | Status     |
|------------|------|----------|-------------|------------|
| 2024-01-15 | R01  | 10       | 2500,00     | Concluído |
| 2024-01-15 | R02  | 8        | 1800,00     | Em Rota   |
```

#### Fórmulas Úteis
1. Total de entregas por status:
```
=SOMASE(E2:E10;"Concluído";C2:C10)
```

2. Valor total por rota:
```
=SOMASE(B2:B10;"R01";D2:D10)
```

### Casos de Uso em Logística

1. **Gestão de Estoque**
   - Localização de produtos
   - Totalização por categoria
   - Controle de mínimo/máximo

2. **Controle de Entregas**
   - Status por região
   - Total de pedidos por cliente
   - Valor médio por rota

### Exercícios Práticos

1. **PROCV - Busca de Informações**
   Crie uma tabela de referência e use PROCV para:
   - Buscar preços de produtos
   - Identificar locais de armazenamento
   - Verificar status de pedidos

2. **SOMASE - Análise de Dados**
   Desenvolva análises para:
   - Total de entregas por região
   - Valor total por cliente
   - Quantidade por tipo de produto

### Erros Comuns a Evitar

1. **No PROCV**
   - Não travar referências ($)
   - Esquecer de usar FALSO para correspondência exata
   - Não atualizar a matriz quando adiciona colunas

2. **No SOMASE**
   - Intervalos de tamanhos diferentes
   - Critérios mal formatados
   - Não considerar células vazias

### Dicas Profissionais

1. **Otimização de PROCV**
   - Use tabelas de referência organizadas
   - Mantenha dados de busca à esquerda
   - Utilize validação de dados para evitar erros

2. **Eficiência no SOMASE**
   - Crie critérios padronizados
   - Use células de referência para critérios
   - Combine com outras funções para análises complexas

🔍 **Dica Avançada**: Para análises mais complexas, considere combinar PROCV com SOMASE usando referências estruturadas de tabela.
