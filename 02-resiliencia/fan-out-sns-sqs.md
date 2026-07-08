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
| **SQS** | Fila (pull) | Desacoplar 1:1, buffer, garantir processamento |
| **SNS** | Pub/Sub (push) | Notificar N assinantes simultaneamente |
| **EventBridge** | Event bus | Orquestrar entre serviços AWS/SaaS, regras complexas |

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
