# Kinesis vs SQS — Streaming vs Fila

## Comparação direta

| Aspecto | SQS | Kinesis Data Streams |
|---|---|---|
| **Padrão** | Fila (mensagem consumida e deletada) | Stream (dados retidos por período) |
| **Múltiplos consumers** | ❌ Não (mensagem vai para 1 consumer) | ✅ Sim (cada consumer lê independente) |
| **Reprocessamento** | ❌ Não (mensagem some após consumo) | ✅ Sim (dados ficam 1-365 dias) |
| **Ordenação** | Melhor esforço (Standard) / Garantida (FIFO) | Garantida por shard |
| **Throughput** | Ilimitado (Standard) / 300 msg/s (FIFO) | Por shard (1 MB/s entrada, 2 MB/s saída) |
| **Ideal para** | Desacoplamento, eventos discretos | IoT, streaming, analytics tempo real |

---

## Quando usar cada um

| Cenário na prova | Solução |
|---|---|
| "Desacoplar serviços", "buffer de escrita" | **SQS** |
| "Não perder mensagem" + processamento único | **SQS** |
| "Streaming", "tempo real", "IoT", "100k eventos/seg" | **Kinesis** |
| "Múltiplos consumers lendo os mesmos dados" | **Kinesis** |
| "Reprocessar dados", "retenção por X dias" | **Kinesis** |
| "Processar exatamente uma vez + ordem" | **SQS FIFO** |

---

## Kinesis Data Streams vs Kinesis Firehose

| | Data Streams | Firehose |
|---|---|---|
| **O que faz** | Ingestão em tempo real + consumers customizados | Entrega automática para destinos |
| **Consumers** | Você constrói (Lambda, EC2, etc.) | Automático (não precisa de consumer) |
| **Destinos** | Qualquer coisa (você decide) | S3, Redshift, OpenSearch, Splunk |
| **Retenção** | 1-365 dias | Não retém (entrega e pronto) |
| **Quando usar** | Processamento customizado em tempo real | "Entregar dados para S3/Redshift sem código" |

> **⚠️ LEMBRETE:** Firehose NÃO entrega para RDS. Destinos: S3, Redshift, OpenSearch, Splunk.

---

## Analogias — fixar a diferença

**SQS = Padaria com senha**
- Você pega um número, é atendido, e o número é jogado fora
- Próximo cliente pega o próximo número
- Ninguém pode usar o mesmo número de novo
- Se a padaria fechar 2h, as senhas que não foram chamadas continuam na fila

**Kinesis = Câmera de segurança gravando**
- A câmera grava tudo continuamente em uma fita
- 3 pessoas diferentes podem assistir a mesma fita ao mesmo tempo
- Cada uma pode pausar, voltar, reassistir
- A fita é mantida por X dias e depois apagada automaticamente

---

## Kinesis vs SNS+SQS fan-out — a confusão perigosa

Ambos entregam dados para múltiplos destinos. A diferença:

| | SNS + SQS (fan-out) | Kinesis Data Streams |
|---|---|---|
| **Como funciona** | SNS faz cópia para cada fila SQS | Consumers lêem do mesmo stream |
| **Após consumir** | Mensagem deletada de cada fila | Dados continuam no stream |
| **Reprocessar** | ❌ Não (já deletou) | ✅ Volta o ponteiro e relê |
| **Retenção máxima** | 14 dias (SQS) | 365 dias (Kinesis) |
| **Gerenciamento** | Zero (serverless) | Precisa dimensionar shards |

**Quando fan-out (SNS+SQS) resolve:**
- Múltiplos destinos + cada um processa e descarta + não precisa reprocessar
- Exemplo: pedido confirmado → email + estoque + faturamento

**Quando só Kinesis resolve:**
- Múltiplos consumers + reprocessamento + retenção longa + streaming contínuo
- Exemplo: 10.000 sensores GPS → mapa + analytics + auditoria (90 dias)

---

## Decisão em 10 segundos na prova

```
1. Precisa "reprocessar" ou "replay" dados?
   → SIM → Kinesis (certeza absoluta)
   → NÃO → continua...

2. É streaming contínuo em tempo real (IoT, GPS, cliques)?
   → SIM → Kinesis
   → NÃO → continua...

3. Múltiplos consumers lendo os MESMOS dados retidos?
   → SIM → Kinesis
   → NÃO → continua...

4. Quer "desacoplar" + "buffer" + processamento simples?
   → SQS

5. Quer "1 evento → N destinos" (sem reprocessamento)?
   → SNS + SQS (fan-out)
```

> **Palavra mágica:** "reprocessar" → Kinesis. Sempre. SQS deleta após consumo.

---

## ⚠️ LEMBRETE para a prova

- "Múltiplos consumers" + "reprocessar" + "streaming" → **Kinesis Data Streams**
- "Desacoplar" + "buffer" + "mensagem processada uma vez" → **SQS**
- "Entregar para S3/Redshift sem gerenciar consumers" → **Kinesis Firehose**
- "1 evento → N destinos" + "garantia" + "sem reprocessar" → **SNS + SQS fan-out**
- "Retenção > 14 dias" → **Kinesis** (SQS máx 14 dias)

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
