# Tópicos de Estudo — AWS SAA-C03

> **Este material é vivo.** Tudo que está aqui surgiu de uma conversa real — uma pergunta respondida, um erro cometido, uma dúvida esclarecida. Nada foi copiado ou inventado. A cada sessão de estudo, novos tópicos são adicionados, erros são corrigidos e o conteúdo evolui. Se você chegou aqui, pode usar este repositório como base para o seu próprio aprendizado — o material reflete o que realmente funciona quando estudado com raciocínio ativo, não memorização passiva.

---

## Por que esse método fez diferença para mim

Ao longo da minha jornada de estudos, sempre aprendi lendo e assistindo vídeoaulas. Nunca consegui aprender fazendo anotações ou resumos — sempre achei trabalhoso demais e o conteúdo simplesmente não ficava quando tentava escrever dessa forma. Meu jeito natural sempre foi absorver ouvindo e lendo, não registrando.

O problema é que isso tem um limite. Para uma prova como o SAA-C03, que testa tomada de decisão arquitetural em cenários com quatro opções plausíveis, não basta absorver — você precisa raciocinar sob pressão e escolher a melhor resposta entre alternativas que parecem igualmente corretas.

O que percebi ao longo desse processo foi algo que não esperava: **eu comecei a escrever naturalmente — sem perceber e sem forçar**.

Não como anotação. Mas como teste do meu próprio entendimento. Para cada questão, preciso colocar em palavras por que escolhi aquela opção, por que as outras estão erradas e o que no enunciado me levou até ela. Escrever virou o meio de descobrir se o que eu achava que tinha entendido era real ou apenas familiar.

E foi aí que as confusões apareceram. Ler e assistir vídeoaulas me davam o conteúdo — mas não me diziam onde meu raciocínio estava errado. Só quando precisei escrever e justificar é que ficou evidente que eu confundia contextos parecidos, aplicava conceitos certos no cenário errado, ou escolhia o serviço correto pelo motivo incorreto. O Kiro mapeou cada uma dessas confusões, corrigiu com precisão e registrou aqui para não se repetir.

Esse repositório é o resultado disso: um jeito de estudar que me fez fazer, pela primeira vez, o que nunca tinha conseguido fazer na vida — registrar o que aprendi. Não porque foi exigido, mas porque fez sentido dentro do processo.

Se você também aprende melhor lendo e ouvindo do que anotando, talvez esse formato funcione para você também: não escreva para resumir — escreva para testar se você realmente entendeu.

---

## Sobre este material

Este repositório é um **diário de aprendizado ativo** para a certificação AWS Certified Solutions Architect - Associate (SAA-C03).

A jornada começou a partir do artigo publicado no blog oficial da AWS Brasil:
**[Pratique exames de certificação AWS com feedback de IA no Kiro](https://aws.amazon.com/pt/blogs/aws-brasil/pratique-exames-de-certificacao-aws-com-feedback-de-ia-no-kiro/)**
— escrito por Marcus Santos e Lucas Leme Lopes, consultores de Professional Services Brasil.

O artigo apresenta o **Kiro Study Buddy**, um power open-source para o Kiro IDE que transforma o estudo passivo em **raciocínio ativo**: em vez de memorizar definições, você pratica justificando suas decisões arquiteturais e recebe feedback baseado na documentação oficial da AWS.

O estudo foi conduzido com apoio do **Kiro** — assistente de IA da AWS integrado ao ambiente de desenvolvimento — como parceiro de estudos: propondo questões com pegadinhas, identificando lacunas de conhecimento, corrigindo conceitos invertidos e mantendo este material sempre atualizado com o progresso real.

O conteúdo não foi copiado de livro nem de curso. Cada arquivo, cada tabela de palavras-chave e cada correção registrada existem porque surgiram naturalmente durante o estudo:

- ✅ Acertos e erros em questões práticas no estilo do exame real
- ✅ Análise do raciocínio por trás de cada resposta, não só o resultado
- ✅ Correções aplicadas no momento da dúvida, com o conceito certo em mãos
- ✅ Revisão contínua dos pontos onde houve confusão

> A ideia central — citada no próprio artigo: **"Você pratica raciocínio ativo, não memorização."** Não basta acertar — é preciso saber por que as outras opções estão erradas.

---

## Contexto — primeiro exame

| Campo | Valor |
|---|---|
| Data | 03/12/2025 |
| Pontuação | 698 / 1000 |
| Mínimo para passar | 720 |
| Resultado | ❌ Reprovado (22 pontos abaixo) |

Resultado por domínio no primeiro exame:

| Domínio | Peso | Resultado |
|---|---|---|
| Design de arquiteturas seguras | 30% | Precisa melhorar |
| Design de arquiteturas resilientes | 26% | Precisa melhorar |
| Design de arquiteturas de alta performance | 24% | ✅ Atende |
| Design de arquiteturas econômicas | 20% | Precisa melhorar |

Este material foi construído focado nesses gaps reais.

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

- [Amazon Cognito](01-seguranca/cognito.md)
- [AWS Organizations, SCPs e RAM](01-seguranca/organizations-scp-ram.md)
- [Bucket Policy no S3](01-seguranca/bucket-policy-s3.md)
- [CloudTrail vs Config vs GuardDuty](01-seguranca/cloudtrail-config-guardduty.md)
- [IAM — Roles, Policies e Users](01-seguranca/iam-roles-policies.md)
- [IAM Identity Center (SSO) e ACM](01-seguranca/iam-identity-center.md)
- [KMS e Criptografia](01-seguranca/kms-criptografia.md)
- [Macie, Inspector e Presigned URL](01-seguranca/macie-inspector.md)
- [Modelo de 3 Camadas de Segurança](01-seguranca/modelo-3-camadas-seguranca.md)
- [Security Group vs NACL](01-seguranca/security-group-vs-nacl.md)
- [Subnet Pública vs Privada](01-seguranca/subnet-publica-vs-privada.md)
- [VPC Endpoints](01-seguranca/vpc-endpoints.md)
- [WAF e Shield — Proteção contra ataques](01-seguranca/waf-shield-ddos.md)

## Domínio 2 — Design de Arquiteturas Resilientes (26%)

- [Amazon MQ — RabbitMQ/ActiveMQ gerenciado](02-resiliencia/amazon-mq.md)
- [Auto Scaling e Availability Zones](02-resiliencia/auto-scaling-azs.md)
- [CloudWatch Alarms — Composite Alarms](02-resiliencia/cloudwatch-alarms.md)
- [Desacoplamento com SQS](02-resiliencia/desacoplamento-sqs.md)
- [Disaster Recovery (DR)](02-resiliencia/disaster-recovery.md)
- [DynamoDB — Resiliência e Auditoria (PITR, Streams)](02-resiliencia/dynamodb-resiliencia.md)
- [Fan-out com SNS + SQS](02-resiliencia/fan-out-sns-sqs.md)
- [Kinesis vs SQS — Streaming vs Fila](02-resiliencia/kinesis-vs-sqs.md)
- [RDS Custom — Acesso ao SO](02-resiliencia/rds-custom.md)
- [RDS Multi-AZ vs Read Replicas](02-resiliencia/multi-az-vs-read-replicas.md)

## Domínio 3 — Design de Arquiteturas de Alta Performance (24%)

- [Amazon Athena e AWS Glue Job Bookmarks](03-alta-performance/athena-glue.md)
- [CloudFront vs Transfer Acceleration vs Global Accelerator](03-alta-performance/cloudfront-vs-transfer-vs-global-accelerator.md)
- [CloudWatch Logs → OpenSearch](03-alta-performance/cloudwatch-logs-opensearch.md)
- [Conteúdo Estático vs Dinâmico](03-alta-performance/conteudo-estatico-vs-dinamico.md)
- [ElastiCache — quando usar](03-alta-performance/elasticache.md)
- [Load Balancers (ALB, NLB, GWLB)](03-alta-performance/load-balancers.md)
- [S3 Replication + EventBridge](03-alta-performance/s3-replication-eventbridge.md)

## Domínio 4 — Design de Arquiteturas Econômicas (20%)

- [Classes de Armazenamento S3](04-custo/classes-armazenamento-s3.md)
- [Migração de Dados — Snowball, DataSync, Direct Connect](04-custo/migracao-dados.md)
- [Modelos de Compra EC2](04-custo/modelos-compra-ec2.md)

## Referência Rápida

- [Comparação de Serviços](referencia/comparacao-servicos.md)
- [Método de Eliminação para a Prova](referencia/metodo-eliminacao.md)
- [Palavras-chave → Serviço (Tabela Mestre)](referencia/palavras-chave.md)

## Simulados

- [Evolução dos Simulados](simulados/README.md)

---

*Material baseado em documentação oficial da AWS*
*Última atualização: 08/07/2026*
