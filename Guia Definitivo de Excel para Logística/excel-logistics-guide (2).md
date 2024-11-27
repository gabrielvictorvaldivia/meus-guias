## Fórmulas Fundamentais

### PROCV (VLOOKUP) para Dados de Produtos

#### Sintaxe Básica
```excel
=PROCV(valor_procurado; matriz_tabela; núm_coluna; FALSO)
```

#### Exemplo Prático
```excel
// Buscar preço de frete por região
=PROCV([@Região];TabelaFretes;2;FALSO)

// Buscar tempo médio de entrega por cidade
=PROCV([@Cidade];TemposEntrega;3;FALSO)
```

**Casos de Uso:**
- Atribuição automática de custos de frete
- Busca de informações de produtos por código
- Verificação de restrições de entrega por região

**⚠️ Armadilhas a Evitar:**
- Não usar PROCV com valores aproximados (VERDADEIRO) em códigos
- Sempre verificar se a tabela de busca está ordenada
- Cuidado com espaços extras nos valores de busca

### SOMASE/SUMIF para Análises Condicionais

#### Sintaxe
```excel
=SOMASE(intervalo_critérios; critério; intervalo_soma)
```

#### Exemplos Logísticos
```excel
// Total de entregas por região
=SOMASE(Entregas[Região];"Sul";Entregas[Quantidade])

// Custos totais por tipo de frete
=SOMASE(Fretes[Tipo];"Expresso";Fretes[Valor])
```

### CONCATENAR para Códigos e Referências

#### Sintaxe
```excel
=CONCATENAR(texto1; texto2; ...)
// ou usando &
=texto1 & texto2 & texto3
```

#### Aplicações em Logística
```excel
// Criar código de rastreamento
="PKT" & TEXT(HOJE();"DDMM") & "-" & [@ID_Pedido]

// Gerar referência de armazenamento
=[@Setor] & [@Corredor] & "-" & [@Posição]
```

### SE (IF) para Decisões Logísticas

#### Sintaxe
```excel
=SE(teste_lógico; valor_se_verdadeiro; valor_se_falso)
```

#### Exemplos Práticos
```excel
// Determinar tipo de frete
=SE([@Peso]>1000;"Dedicado";
  SE([@Peso]>100;"Fracionado";"Expresso"))

// Avaliar nível de serviço
=SE([@Tempo_Entrega]<=[@Prazo];"Dentro SLA";"Fora SLA")
```

### ÍNDICE/CORRESP (INDEX/MATCH)

#### Sintaxe
```excel
=ÍNDICE(matriz;CORRESP(valor_procurado;matriz_procura;0))
```

#### Exemplo Avançado
```excel
// Busca bidirecional de tarifa
=ÍNDICE(TarifasFrete;
  CORRESP([@Origem];Origens;0);
  CORRESP([@Destino];Destinos;0))
```

### SOMASES/SUMIFS para Análises Multicritério

#### Sintaxe
```excel
=SOMASES(intervalo_soma; critério1; valor1; critério2; valor2; ...)
```

#### Aplicações Complexas
```excel
// Total de entregas por região e período
=SOMASES(Entregas[Quantidade];
  Entregas[Região];"Sul";
  Entregas[Data];">="&[@Data_Inicial];
  Entregas[Data];"<="&[@Data_Final])

// Custo total por tipo de veículo e cliente
=SOMASES(Fretes[Valor];
  Fretes[Tipo_Veículo];[@Veículo];
  Fretes[Cliente];[@Cliente])
```

### Dicas de Profissionais

> "Sempre use ÍNDICE/CORRESP em vez de PROCV quando precisar de flexibilidade na busca. É mais versátil e permite buscas à esquerda." 
> - Carlos Mendes, Analista de Logística Sênior

> "Crie uma biblioteca de fórmulas comentadas em uma aba separada. Isso ajuda na manutenção e no treinamento de novos membros da equipe."
> - Marina Santos, Gerente de Operações

### Exercício Prático

Crie uma planilha de controle de entregas que:
1. Busque automaticamente o custo do frete por região
2. Calcule o tempo de atraso/adiantamento
3. Determine o nível de serviço
4. Gere códigos de rastreamento únicos
5. Calcule totais por região e período

**⚠️ Notas Importantes:**
- Sempre documente fórmulas complexas
- Use nomes definidos para intervalos frequentemente utilizados
- Evite fórmulas aninhadas muito profundas
- Considere usar tabelas dinâmicas para análises frequentes

### Recursos Adicionais
- Biblioteca de Fórmulas Excel para Logística (anexo)
- Templates de Controle de Operações
- Guia de Melhores Práticas em Fórmulas

Deseja prosseguir para a próxima seção sobre Análise de Dados Logísticos?
