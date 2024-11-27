## Automatização

### Macros Essenciais para Logística

#### 1. Atualização de Dados
```vba
Sub AtualizarDadosLogistica()
    ' Desabilitar para performance
    Application.ScreenUpdating = False
    Application.Calculation = xlCalculationManual
    
    ' Atualizar dados
    Sheets("RAW_Dados").Select
    Range("TabelaDados").RefreshTable
    
    ' Recalcular KPIs
    Sheets("ANA_KPIs").Calculate
    
    ' Atualizar relatórios
    Sheets("DAS_Principal").PivotTables("PV_Performance").RefreshTable
    
    ' Restaurar configurações
    Application.Calculation = xlCalculationAutomatic
    Application.ScreenUpdating = True
    
    ' Confirmar atualização
    MsgBox "Dados atualizados em " & Format(Now(), "dd/mm/yyyy hh:mm"), vbInformation
End Sub
```

#### 2. Geração de Relatórios
```vba
Sub GerarRelatorioEntregas()
    Dim ws As Worksheet
    Dim rng As Range
    
    ' Criar novo relatório
    Set ws = Sheets.Add
    ws.Name = "REL_Entregas_" & Format(Date, "yyyymmdd")
    
    ' Copiar dados filtrados
    Sheets("ANA_Entregas").Range("TabelaEntregas").AutoFilter _
        Field:=1, Criteria1:=Date
    
    ' Formatar relatório
    With ws
        .Range("A1").Value = "RELATÓRIO DE ENTREGAS DIÁRIAS"
        .Range("A2").Value = "Data: " & Format(Date, "dd/mm/yyyy")
        ' Adicionar mais formatação conforme necessário
    End With
    
    ' Enviar por email
    EnviarRelatorioEmail ws.Name
End Sub
```

### Power Query para Importação

#### 1. Configuração Básica
```
let
    Fonte = Excel.Workbook(File.Contents("dados_logistica.xlsx"), null, true),
    Entregas = Fonte{[Item="Entregas",Kind="Table"]}[Data],
    
    // Limpeza de dados
    LimparDados = Table.TransformColumnTypes(
        Entregas,
        {{"Data", type date}, 
         {"Valor", type number},
         {"Quantidade", Int64.Type}}
    ),
    
    // Adicionar métricas calculadas
    AdicionarMetricas = Table.AddColumn(
        LimparDados, 
        "LeadTime",
        each [DataEntrega] - [DataPedido]
    )
in
    AdicionarMetricas
```

#### 2. Transformações Comuns
```
// Padronizar nomes de cidades
= Table.ReplaceValue(
    Tabela,
    "SAO PAULO",
    "São Paulo",
    Replacer.ReplaceText,
    {"Cidade"}
)

// Calcular métricas
= Table.AddColumn(
    Tabela,
    "CustoPorKm",
    each [CustoTotal]/[Distancia],
    type number
)
```

### Atualizações Automáticas

#### 1. Timer VBA
```vba
Private Sub Workbook_Open()
    ' Configurar timer para atualização
    Application.OnTime Now + TimeValue("01:00:00"), "AtualizarDados"
End Sub

Sub AtualizarDados()
    ' Executar atualização
    RefreshAllData
    
    ' Agendar próxima atualização
    Application.OnTime Now + TimeValue("01:00:00"), "AtualizarDados"
End Sub
```

#### 2. Triggers de Eventos
```vba
Private Sub Worksheet_Change(ByVal Target As Range)
    ' Verificar se alteração foi em célula relevante
    If Not Intersect(Target, Range("TabelaDados")) Is Nothing Then
        Application.EnableEvents = False
        
        ' Atualizar cálculos dependentes
        AtualizarCalculos
        
        Application.EnableEvents = True
    End If
End Sub
```

### Relatórios Programados

#### 1. Geração Automática
```vba
Sub GerarRelatoriosDiarios()
    Dim DataInicio As Date
    Dim DataFim As Date
    
    ' Definir período
    DataInicio = DateSerial(Year(Date), Month(Date), 1)
    DataFim = Date
    
    ' Gerar relatórios
    GerarRelatorioEntregas DataInicio, DataFim
    GerarRelatorioCustos DataInicio, DataFim
    GerarRelatorioPerformance DataInicio, DataFim
    
    ' Enviar por email
    EnviarRelatorioPorEmail
End Sub
```

#### 2. Distribuição Automatizada
```vba
Sub EnviarRelatorioPorEmail()
    Dim OutApp As Object
    Dim OutMail As Object
    
    Set OutApp = CreateObject("Outlook.Application")
    Set OutMail = OutApp.CreateItem(0)
    
    With OutMail
        .To = "equipe@empresa.com"
        .Subject = "Relatório Logística - " & Format(Date, "dd/mm/yyyy")
        .Body = "Segue relatório automático de logística."
        .Attachments.Add ActiveWorkbook.FullName
        .Send
    End With
End Sub
```

### Dicas de Profissionais

> "Sempre inclua tratamento de erros em suas automações. Um erro não tratado pode causar horas de retrabalho."
> - Paulo Silva, Desenvolvedor VBA Sênior

> "Mantenha um log de execuções automáticas. Isso facilita muito o troubleshooting."
> - Maria Costa, Analista de Sistemas

**⚠️ Notas Importantes:**
- Sempre faça backup antes de implementar automações
- Teste extensivamente em ambiente controlado
- Documente todas as macros e processos
- Mantenha um sistema de controle de versão

### Recursos Adicionais
- Biblioteca de Macros Logísticas
- Templates de Power Query
- Guia de Troubleshooting
- Manual de Manutenção

Deseja prosseguir para a última seção sobre Resolução de Problemas?
