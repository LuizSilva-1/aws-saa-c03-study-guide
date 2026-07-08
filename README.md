# Tópicos de Estudo — AWS SAA-C03

> Material de estudo para a certificação AWS Solutions Architect Associate (SAA-C03).
> Cada arquivo cobre um tópico específico. Organizado por domínio.

---

## Sobre o Exame

| Detalhe | Valor |
|---|---|
| **Exame** | AWS Certified Solutions Architect - Associate (SAA-C03) |
| **Questões** | 50 pontuadas + 15 não pontuadas = 65 total |
| **Tempo** | 130 minutos (~2 min por questão) |
| **Nota de corte** | 720 / 1000 |

---

## Domínio 1 — Design de Arquiteturas Seguras (30%)

- [Security Group vs NACL](01-seguranca/security-group-vs-nacl.md)
- [VPC Endpoints](01-seguranca/vpc-endpoints.md)
- [KMS e Criptografia](01-seguranca/kms-criptografia.md)
- [IAM — Roles, Policies e Users](01-seguranca/iam-roles-policies.md)
- [Modelo de 3 Camadas de Segurança](01-seguranca/modelo-3-camadas-seguranca.md)
- [Subnet Pública vs Privada](01-seguranca/subnet-publica-vs-privada.md)
- [Bucket Policy no S3](01-seguranca/bucket-policy-s3.md)
- [WAF e Shield — Proteção contra ataques](01-seguranca/waf-shield-ddos.md)
- [CloudTrail vs Config vs GuardDuty](01-seguranca/cloudtrail-config-guardduty.md)
- [AWS Organizations, SCPs e RAM](01-seguranca/organizations-scp-ram.md)

## Domínio 2 — Design de Arquiteturas Resilientes (26%)

- [RDS Multi-AZ vs Read Replicas](02-resiliencia/multi-az-vs-read-replicas.md)
- [Desacoplamento com SQS](02-resiliencia/desacoplamento-sqs.md)
- [Fan-out com SNS + SQS](02-resiliencia/fan-out-sns-sqs.md)
- [Auto Scaling e Availability Zones](02-resiliencia/auto-scaling-azs.md)

## Domínio 3 — Design de Arquiteturas de Alta Performance (24%)

- [CloudFront vs Transfer Acceleration vs Global Accelerator](03-alta-performance/cloudfront-vs-transfer-vs-global-accelerator.md)
- [Conteúdo Estático vs Dinâmico](03-alta-performance/conteudo-estatico-vs-dinamico.md)
- [ElastiCache — quando usar](03-alta-performance/elasticache.md)
- [Load Balancers (ALB, NLB, GWLB)](03-alta-performance/load-balancers.md)

## Domínio 4 — Design de Arquiteturas Econômicas (20%)

- [Modelos de Compra EC2](04-custo/modelos-compra-ec2.md)
- [Classes de Armazenamento S3](04-custo/classes-armazenamento-s3.md)

## Referência Rápida

- [Palavras-chave → Serviço (Tabela Mestre)](referencia/palavras-chave.md)
- [Método de Eliminação para a Prova](referencia/metodo-eliminacao.md)
- [Comparação de Serviços](referencia/comparacao-servicos.md)

---

*Material baseado em documentação oficial da AWS*
*Última atualização: 08/07/2026*
