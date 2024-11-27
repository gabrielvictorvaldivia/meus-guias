# Guia Excel para Analista de Logística Júnior
## PÁGINA 2: FUNDAMENTOS DE ORGANIZAÇÃO

### Estruturação de Planilhas
1. **Organização por Abas**
   - Dados Brutos
   - Análises
   - Relatórios
   - Dashboards
   - Documentação

2. **Estrutura Padrão de Dados**
```
| Data       | Código | Descrição | Quantidade | Status    | Responsável |
|------------|--------|-----------|------------|-----------|-------------|
| 2024-01-01 | A001   | Carga 1   | 100        | Entregue  | João Silva  |
| 2024-01-01 | A002   | Carga 2   | 150        | Em Rota   | Maria Souza |
```

### Nomenclatura Padrão
1. **Nomes de Arquivos**
   - LOG_Estoque_2024_01.xlsx
   - LOG_Rotas_2024_Q1.xlsx
   - LOG_KPI_2024_Mensal.xlsx

2. **Nomes de Abas**
   - DB_[Tipo de Dado]
   - REL_[Tipo de Relatório]
   - DASH_[Nome do Dashboard]

### Boas Práticas Iniciais
1. **Consistência de Dados**
   - Use formatos de data padronizados (AAAA-MM-DD)
   - Mantenha códigos em formato texto
   - Padronize unidades de medida
   - Utilize nomenclatura consistente

2. **Segurança**
   - Bloqueie células com fórmulas
   - Proteja abas importantes
   - Mantenha backup diário
   - Versione os arquivos

### Formatação Básica
1. **Células e Textos**
   - Alinhamento: Números à direita, texto à esquerda
   - Fonte: Calibri 11 ou Arial 10
   - Cores: Máximo 3 cores por planilha
   - Bordas: Apenas quando necessário

2. **Formatação de Dados**
   - Números: Duas casas decimais
   - Datas: DD/MM/AAAA
   - Percentuais: Com símbolo %
   - Moeda: R$ #.##0,00

### Exercício Prático
Crie uma planilha de controle de entregas seguindo o modelo:
```
| Data Envio | Código | Destinatário | Valor R$ | Status    | Prazo    |
|------------|--------|--------------|----------|-----------|----------|
| 2024-01-15 | E001   | Cliente A    | 1500,00  | Pendente  | 3 dias   |
| 2024-01-16 | E002   | Cliente B    | 2300,00  | Em Rota   | 2 dias   |
```

### Erros Comuns a Evitar
1. Misturar dados de diferentes naturezas na mesma planilha
2. Usar formatação inconsistente
3. Não documentar alterações importantes
4. Deixar células em branco em meio aos dados
5. Usar cores sem critério definido

### Dicas Profissionais
- Mantenha um arquivo modelo (template) para cada tipo de análise
- Documente todas as alterações significativas
- Use validação de dados para evitar erros de entrada
- Estabeleça um padrão de cores para sua equipe
- Crie um índice de abas para arquivos complexos

🔍 **Lembre-se**: A organização é a base para análises eficientes e confiáveis.
