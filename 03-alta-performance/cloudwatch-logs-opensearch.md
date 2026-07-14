# CloudWatch Logs → OpenSearch — Entrega em Tempo Real

## O problema

Empresa quer enviar logs do CloudWatch Logs para o Amazon OpenSearch Service em **tempo quase real** com **menor overhead operacional**.

---

## Duas soluções válidas

### 1. Subscription Filter (mais simples)

```
CloudWatch Logs
    ↓ Subscription Filter (configuração, sem código)
Amazon OpenSearch Service
```

- Configuração nativa, sem código adicional
- Ideal para casos simples, sem necessidade de transformação
- Entrega em tempo quase real

### 2. Kinesis Data Firehose (mais flexível)

```
CloudWatch Logs
    ↓ Subscription Filter
Kinesis Data Firehose
    ↓ (opcional: transformação via Lambda)
Amazon OpenSearch Service
```

- Permite transformar/filtrar logs antes de entregar
- Suporta buffering e retries automáticos
- Ideal quando os logs precisam de enriquecimento

---

## O que NÃO usar

| Opção | Por que não |
|---|---|
| **Lambda** | Código para manter = overhead operacional |
| **Kinesis Agent em cada servidor** | Instalar agente em cada EC2 = overhead alto |
| **Kinesis Data Streams** | Exige consumers manuais = mais complexo |

---

## Palavras-chave na prova

| Cenário | Solução |
|---|---|
| "logs CloudWatch → OpenSearch, tempo real, menor overhead" | **Subscription Filter** ou **Kinesis Firehose** |
| "transformar logs antes de entregar" | **Kinesis Firehose + Lambda** |
| "sem instalar agente" | **Subscription Filter** |
| "menor overhead, logs simples" | **Subscription Filter** |

---

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
