# Desacoplamento com SQS

## O problema

Banco de dados ou serviço não aguenta o volume de **escritas** nos horários de pico. Transações são perdidas ou retornam timeout.

---

## A solução — Queue-based Decoupling

```
Antes:  App → RDS (escrita direta — banco sufoca no pico)

Depois: App → SQS (buffer) → Lambda (consumer) → RDS (ritmo do banco)
```

---

## Por que funciona

| Benefício | Explicação |
|---|---|
| **Absorve picos** | SQS aceita milhões de mensagens/segundo — nunca perde |
| **Zero perda** | Mensagens ficam na fila até serem processadas com sucesso |
| **Banco no ritmo dele** | Consumer lê da fila na velocidade que o RDS aguenta |
| **Escala automática** | Lambda escala com o tamanho da fila |
| **Zero overhead** | SQS + Lambda = serverless, sem gerenciar infra |

---

## Sinais na prova

| Palavra-chave no cenário | Indica desacoplamento com SQS |
|---|---|
| "banco atingindo limite de conexões" | ✅ |
| "não pode perder mensagens/transações" | ✅ |
| "absorver picos sem intervenção manual" | ✅ |
| "produtor mais rápido que consumer" | ✅ |
| "milhares de escritas por segundo" | ✅ |

---

## Erro comum

Confundir com Read Replica. Se o problema é de **escrita** (INSERT, UPDATE), Read Replica NÃO ajuda — réplicas são somente leitura.

| Problema | Solução |
|---|---|
| Muita **escrita** sobrecarregando o banco | **SQS** (desacoplar) |
| Muita **leitura** sobrecarregando o banco | **Read Replica** (offload) |

---

## Consumer: Lambda vs EC2

| | Lambda | EC2 |
|---|---|---|
| **Overhead** | Zero (serverless) | Alto (gerenciar instâncias) |
| **Escala** | Automática | Precisa de Auto Scaling |
| **Custo fora do pico** | ~Zero | Instância rodando = pagando |
| **Ideal para** | Carga variável | Carga constante alta |

Na prova, se pedir "menor overhead operacional" → **Lambda**.

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
