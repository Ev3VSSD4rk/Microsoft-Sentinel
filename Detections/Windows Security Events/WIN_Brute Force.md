
# Query para Detectar Brute Force em Windows

Esta query é usada para detectar ataques de brute force no Azure Sentinel.

## Descrição

O objetivo desta query é identificar padrões típicos de ataques de brute force baseados em falhas de login sucessivas.

## Exemplo de Regra

```kql
SecurityEvent
| where EventID == 4625  // Falha de login
| where TimeGenerated > ago(1h)
| summarize FailedAttempts = count() by TargetUserName, bin(TimeGenerated, 5m)
| where FailedAttempts > 10
| project TargetUserName, FailedAttempts, TimeGenerated
