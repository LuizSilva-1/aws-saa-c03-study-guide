# VPC Endpoints

## O que são

Permitem que recursos na VPC acessem serviços AWS **sem sair para a internet pública**. O tráfego fica dentro da rede AWS.

---

## Tipos

| Tipo | Serviços suportados | Custo | Como funciona |
|---|---|---|---|
| **Gateway Endpoint** | S3 e DynamoDB (somente) | Gratuito | Entrada na route table da subnet |
| **Interface Endpoint** | Todos os outros serviços | Pago (hora + GB) | ENI com IP privado na subnet |

> **⚠️ LEMBRETE:** Gateway Endpoint = SOMENTE S3 e DynamoDB. Qualquer outro serviço (CloudWatch, SQS, KMS, SNS, etc.) = Interface Endpoint. Se a prova oferecer "Gateway Endpoint para CloudWatch" → está ERRADO.

---

## Quando usar

| Cenário na prova | Solução |
|---|---|
| "Sem acesso à internet" + "acessar S3" | Gateway Endpoint (gratuito) |
| "Sem acesso à internet" + "acessar DynamoDB" | Gateway Endpoint (gratuito) |
| "Sem acesso à internet" + "acessar SQS/SNS/KMS/etc" | Interface Endpoint (pago) |
| "Tráfego não pode sair da VPC" | VPC Endpoint (tipo depende do serviço) |

---

## Gateway Endpoint vs Interface Endpoint — decisão rápida

```
Precisa acessar S3 ou DynamoDB sem internet?
  → Gateway Endpoint (gratuito, configurado na route table)

Precisa acessar qualquer OUTRO serviço AWS sem internet?
  → Interface Endpoint (pago, cria ENI na subnet)
```

---

## Observações para a prova

- Gateway Endpoint é configurado na **route table** (não tem IP próprio)
- Interface Endpoint cria uma **ENI** (Elastic Network Interface) com IP privado na subnet
- VPC Endpoint ≠ IAM Role. Endpoint resolve **rede**, Role resolve **autenticação**
- Pode combinar os dois: IAM Role (sem access key) + VPC Endpoint (sem internet)

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
