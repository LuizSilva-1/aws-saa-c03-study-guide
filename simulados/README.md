# 📊 Simulados — Evolução dos Estudos

> Registro de todos os simulados realizados, mostrando evolução do aprendizado ao longo do tempo.
> Questões geradas por IA baseadas em documentação oficial AWS — não contém conteúdo proprietário de exames.

---

## Resumo de Evolução

| Data | Simulado | Score | Resultado | Observação |
|---|---|---|---|---|
| 08/07/2026 | Simulado 1 (5 questões) | 1/5 (20%) | ❌ FAIL | Primeiro contato — muitas confusões conceituais |
| 08/07/2026 | Simulado 2 (5 questões) | 4/5 (80%) | ✅ PASS | Após estudo dirigido — grande evolução |
| 09/07/2026 | Simulado 3 (10 questões) | 6/10 (60%) | ❌ FAIL | Segurança melhorou! Erros em conceitos novos (Savings Plans, NAT, Lambda@Edge) |
| 09/07/2026 | Simulado 4 (5 questões) | 4/5 (80%) | ✅ PASS | Conceitos reforçados fixaram. Único erro: não leu "personalizado" (Lambda@Edge) |

---

## Simulado 1 — 08/07/2026 (5 questões)

**Score: 1/5 (20%) — FAIL**

| Q# | Domínio | Resultado | Conceito testado | Erro |
|---|---|---|---|---|
| 1 | Segurança | ❌ | KMS CMK vs AWS Managed Keys | Escolheu AWS Managed (não dá controle) |
| 2 | Resiliência | ❌ | Multi-AZ + AZs para EC2 | Escolheu Read Replica + só 2 AZs |
| 3 | Alta Performance | ❌ | CloudFront vs Transfer Acceleration | Confundiu upload com download |
| 4 | Custo | ❌ | Spot Instances | Sabia a resposta mas mudou de ideia |
| 5 | Segurança | ✅ | VPC Gateway Endpoint + IAM Role | Acertou modelo de 3 camadas |

**Lições aprendidas:**
- Transfer Acceleration = upload. CloudFront = download.
- Multi-AZ standby NÃO aceita conexões
- Confiar na intuição quando os sinais apontam para Spot

---

## Simulado 2 — 08/07/2026 (5 questões)

**Score: 4/5 (80%) — PASS**

| Q# | Domínio | Resultado | Conceito testado | Observação |
|---|---|---|---|---|
| 1 | Resiliência | ✅ | Desacoplamento SQS + Lambda | Conceito fixado após cenário socrático |
| 2 | Alta Performance | ✅ | Global Accelerator | IPs anycast + TCP/UDP + failover |
| 3 | Custo | ✅ | Spot Instances para ML | Confiou na intuição desta vez |
| 4 | Resiliência | ❌ | Read Replica para analytics | Confundiu com ElastiCache |
| 5 | Alta Performance | ✅ | CloudFront + TTL | Conteúdo semi-estático |

**Lições aprendidas:**
- Relatórios/SELECT = LEITURA → Read Replica (não cache)
- ElastiCache = queries repetitivas idênticas. Analytics variável = Read Replica

---

## Simulado 3 — 09/07/2026 (10 questões)

**Score: 6/10 (60%) — FAIL**

| Q# | Domínio | Resultado | Conceito testado | Observação |
|---|---|---|---|---|
| 1 | Segurança | ✅ | WAF para SQL injection + bots | Identificou WAF + ALB corretamente |
| 2 | Segurança | ✅ | SCP para restringir toda a organização | "Nem admin pode" = SCP |
| 3 | Segurança | ✅ | S3 Block Public Access no nível da conta | Boa eliminação — releu e notou "conta" sem Organizations |
| 4 | Segurança | ❌ | NAT Gateway para subnet privada | Escolheu IGW — não leu "subnet privada" |
| 5 | Resiliência | ✅ | SQS FIFO para exactly-once + ordem | "Processado mais de uma vez" = FIFO |
| 6 | Resiliência | ❌ | Read Replica sob demanda (criar/deletar) | Não sabia que pode criar/deletar sob demanda |
| 7 | Custo | ✅ | S3 Lifecycle (Standard → IA → Glacier Flexible) | Identificou velocidades de recuperação corretas |
| 8 | Custo | ❌ | Compute Savings Plans vs Convertible RI | Não sabia que SP abrange Fargate/Lambda |
| 9 | Resiliência | ✅ | SQS + ASG para processamento longo de vídeo | Corrigiu de Lambda (15 min) para SQS + ASG |
| 10 | Alta Performance | ❌ | Lambda@Edge vs Global Accelerator | GA melhora latência mas não chega a <100ms cross-continent |

**Lições aprendidas:**
- "Subnet privada" + "internet" = NAT Gateway (NUNCA IGW direto)
- Read Replica pode ser criada/deletada sob demanda (padrão de custo)
- Compute Savings Plans abrange EC2 + Fargate + Lambda (mais flexível que Convertible RI)
- Lambda@Edge = compute personalizado no edge. Global Accelerator = otimiza caminho de rede (não elimina distância)
- Lambda tem limite de 15 minutos — processamento longo precisa de EC2/Fargate

**Evolução em Segurança:** 3/4 (75%) — grande melhora comparado ao Simulado 1 (1/2 = 50%)

---

## Gráfico de Evolução

```
Score %
100 |
 90 |
 80 |          ★ S2 (80%)          ★ S4 (80%)
 70 |     ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─  Nota de corte (72%)
 60 |                    ★ S3 (60%)
 50 |
 40 |
 30 |
 20 | ★ S1 (20%)
 10 |
  0 |___________________________________
     S1        S2        S3        S4
```

---

## Simulado 4 — 09/07/2026 (5 questões)

**Score: 4/5 (80%) — PASS**

| Q# | Domínio | Resultado | Conceito testado | Observação |
|---|---|---|---|---|
| 1 | Segurança | ✅ | NAT Gateway + Gateway Endpoint + Interface Endpoint | Identificou que CloudWatch precisa de Interface Endpoint (não Gateway) |
| 2 | Alta Performance | ❌ | Lambda@Edge vs CloudFront cache | Não leu "personalizado" — escolheu cache com TTL |
| 3 | Custo | ✅ | Compute SP + Spot + On-Demand (scheduled) | Combinação perfeita para 3 workloads diferentes |
| 4 | Resiliência | ✅ | DR Warm Standby (RTO 30 min, RPO 5 min) | Boa eliminação das outras estratégias |
| 5 | Resiliência | ✅ | Kinesis Data Streams para streaming IoT | Múltiplos consumers + retenção 7 dias |

**Lições aprendidas:**
- ANTES de escolher CloudFront cache, perguntar: "dois usuários receberiam a mesma resposta?" Se NÃO → Lambda@Edge
- Gateway Endpoint = S3 e DynamoDB APENAS. Qualquer outro serviço = Interface Endpoint
- DR strategies: Backup&Restore < Pilot Light < Warm Standby < Active-Active (custo e RTO)
- Kinesis vs SQS: streaming + múltiplos consumers + retenção → Kinesis

---

*Questões geradas por IA para estudo — não representam conteúdo oficial de exames AWS*
