# DynamoDB — Resiliência e Auditoria

## Point-in-Time Recovery (PITR)

**O que é:** Backup contínuo e automático do DynamoDB que permite restaurar a tabela para **qualquer segundo** dentro dos últimos **35 dias**.

**RPO (Recovery Point Objective):** Segundos — muito abaixo de qualquer requisito como "RPO < 5 horas".

```
Backup sob demanda:    |-------snapshot-------|  RPO = horas (manual)
PITR:                  |||||||||||||||||||||||||  RPO = segundos (contínuo)
```

**Quando habilitar:** Sempre que o cenário mencionar RPO baixo + DynamoDB.

---

## DynamoDB Streams

**O que é:** Registro sequencial e imutável de todas as mudanças (inserção, atualização, exclusão) em uma tabela DynamoDB.

**Para que serve:**
- Auditoria — quem mudou o quê e quando
- Acionar Lambda em tempo real quando um item muda
- Replicação para outra região (Global Tables)
- Retenção: **24 horas** por padrão (para auditoria mais longa → exportar para S3 via Lambda)

```
Tabela DynamoDB
    ↓ mudança
DynamoDB Stream
    ↓
Lambda (processa em tempo real)
    ↓
S3 ou CloudWatch Logs (retenção longa)
```

---

## Backups sob demanda vs PITR

| | Backup sob demanda | PITR |
|---|---|---|
| **Tipo** | Manual, pontual | Contínuo, automático |
| **RPO** | Horas (depende da frequência) | Segundos |
| **Retenção** | Indefinida | 35 dias |
| **Quando usar** | Snapshot antes de mudança crítica | Requisito de RPO baixo |

> Regra: "RPO < 5 horas" + DynamoDB → **PITR**, não backup sob demanda.

---

## Escalabilidade automática do DynamoDB

- DynamoDB escala automaticamente capacidade de leitura e escrita
- Sem provisionar servidores, sem downtime
- Ideal para cargas com variação significativa (sazonalidade, picos)

---

## Palavras-chave na prova

| Palavra-chave | Solução |
|---|---|
| "RPO baixo" + DynamoDB | **DynamoDB PITR** |
| "auditoria de mudanças" + DynamoDB | **DynamoDB Streams** |
| "reagir a mudanças em tempo real" + DynamoDB | **DynamoDB Streams + Lambda** |
| "backups manuais" | Backup sob demanda (não garante RPO) |
| "NoSQL + escala automática + variação sazonal" | **DynamoDB** |

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
