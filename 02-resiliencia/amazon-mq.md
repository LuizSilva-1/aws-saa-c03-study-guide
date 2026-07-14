# Amazon MQ

## O que é

Serviço **gerenciado** de broker de mensagens compatível com protocolos abertos: RabbitMQ e ActiveMQ.

Usado quando a empresa já tem uma aplicação com fila RabbitMQ ou ActiveMQ e quer migrar para AWS **sem reescrever o código**.

---

## Quando usar Amazon MQ vs SQS/SNS

| Situação | Serviço |
|---|---|
| Aplicação nova na AWS | **SQS / SNS** (nativo, serverless, mais simples) |
| Migração de fila existente (RabbitMQ, ActiveMQ) | **Amazon MQ** (compatibilidade de protocolo) |
| Precisa de AMQP, MQTT, STOMP, OpenWire | **Amazon MQ** |
| Não quer gerenciar broker em EC2 | **Amazon MQ** |

> Regra: "RabbitMQ", "ActiveMQ", "AMQP", "migrar fila existente" → **Amazon MQ**

---

## Alta disponibilidade — configuração ativo/standby

```
AZ-a: Broker ATIVO (processa mensagens)
AZ-b: Broker STANDBY (em espera)
      ↕ replicação síncrona
Se AZ-a cair → Standby assume automaticamente
```

- Failover automático entre AZs
- Dados replicados de forma síncrona
- Endereço do broker permanece o mesmo para a aplicação

---

## Pegadinha do exame

**Auto Scaling Group para o broker RabbitMQ ≠ alta disponibilidade com baixa complexidade**

ASG em EC2 para broker aumenta complexidade operacional. Amazon MQ resolve com **um único serviço gerenciado**.

---

## Padrão de migração típico no exame

```
ANTES (on-premises / EC2):
  RabbitMQ em EC2 → App em EC2 → PostgreSQL em EC2

DEPOIS (solução correta):
  Amazon MQ ativo/standby → App em EC2 (ASG Multi-AZ) → RDS Multi-AZ
```

Essa combinação substitui todos os três componentes auto-gerenciados por serviços gerenciados.

---

## Palavras-chave na prova

| Palavra-chave | Serviço |
|---|---|
| "RabbitMQ", "ActiveMQ" | **Amazon MQ** |
| "migrar fila existente" | **Amazon MQ** |
| "AMQP", "STOMP", "MQTT", "OpenWire" | **Amazon MQ** |
| "nova fila na AWS" | **SQS** |
| "broker gerenciado + HA" | **Amazon MQ ativo/standby** |

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
