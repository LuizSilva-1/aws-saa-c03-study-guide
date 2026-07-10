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

## ⚠️ LEMBRETE para a prova

- "Múltiplos consumers" + "reprocessar" + "streaming" → **Kinesis Data Streams**
- "Desacoplar" + "buffer" + "mensagem processada uma vez" → **SQS**
- "Entregar para S3/Redshift sem gerenciar consumers" → **Kinesis Firehose**

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
