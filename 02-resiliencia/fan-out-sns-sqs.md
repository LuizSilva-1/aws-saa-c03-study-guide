# Fan-out com SNS + SQS

## O problema

Um evento (ex: pedido confirmado) precisa notificar **múltiplos serviços** simultaneamente com **garantia de entrega**.

---

## A solução — padrão Fan-out

```
Evento (pedido confirmado)
    ↓
SNS Topic
    ├→ SQS Fila 1 → Serviço de Estoque
    ├→ SQS Fila 2 → Serviço de Email
    └→ SQS Fila 3 → Serviço de Analytics
```

---

## Por que SNS + SQS e não só SNS?

| Abordagem | Se o destino estiver fora do ar... |
|---|---|
| **SNS puro** | ❌ Perde a mensagem (push falha) |
| **SNS + SQS** | ✅ Mensagem fica na fila esperando o serviço voltar |

A SQS atua como **buffer durável** entre o SNS e cada serviço consumidor.

---

## Palavras-chave na prova

| Cenário | Solução |
|---|---|
| "Notificar múltiplos serviços" + "garantia de entrega" | **SNS + múltiplas SQS** |
| "Notificar múltiplos serviços" + "sem necessidade de garantia" | **SNS puro** ou **EventBridge** |
| "Processar em ordem + garantia" | **SQS FIFO** |
| "Um evento → uma ação" | **SQS simples** (sem fan-out) |

---

## SNS vs SQS vs EventBridge

| Serviço | Padrão | Use quando... |
|---|---|---|
| **SQS Standard** | Fila (pull) | Desacoplar 1:1, buffer, garantir processamento (sem ordem garantida) |
| **SQS FIFO** | Fila ordenada (pull) | Desacoplar 1:1 + ordem exata + exactly-once delivery |
| **SNS** | Pub/Sub (push) | Notificar N assinantes simultaneamente |
| **EventBridge** | Event bus | Orquestrar entre serviços AWS/SaaS, regras complexas |

---

## SQS Standard vs SQS FIFO

| | SQS Standard | SQS FIFO |
|---|---|---|
| **Ordem** | ❌ Não garantida | ✅ Garantida (First In, First Out) |
| **Duplicatas** | Possível | ❌ Exactly-once (sem duplicatas) |
| **Throughput** | Ilimitado | Até 300 msg/s (3.000 com batching) |
| **Quando usar** | Ordem não importa | Ordem crítica (pedidos, transações) |

> **Regra:** "ordem exata" ou "processar na sequência que chegou" → **SQS FIFO**

---

## EventBridge — por que não resolve garantia de entrega

EventBridge roteia eventos entre serviços mas **não tem fila de buffer**. Se o destino estiver fora do ar no momento do evento, a mensagem pode ser perdida. Para garantia de entrega → SQS.

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
