# 🃏 Flashcards — SAA-C03

> Clique em cada pergunta para revelar a resposta. Revise regularmente para fixar os conceitos.

---

## Domínio 1 — Segurança (30%)

<details>
<summary><strong>Qual a diferença entre Security Group (stateful) e NACL (stateless)?</strong></summary>

- **Security Group:** Se libera entrada na porta 80, a resposta sai automaticamente (não precisa regra de saída para retorno).
- **NACL:** Se libera entrada na porta 80, PRECISA criar regra de saída nas portas efêmeras (1024-65535) para a resposta voltar.
- Security Group só permite (não tem DENY). NACL permite E nega.
- Para bloquear um IP específico → NACL (único com DENY).
</details>

<details>
<summary><strong>EC2 precisa acessar S3 sem access key. O que usar?</strong></summary>

**IAM Role** anexada à instância EC2. Nunca colocar access keys em código ou na instância.
</details>

<details>
<summary><strong>Qual a diferença entre KMS CMK, AWS Managed Key e CloudHSM?</strong></summary>

| Tipo | Quando usar |
|---|---|
| **KMS CMK** | "Controle sobre chaves" + "menor overhead" |
| **AWS Managed Key** | Zero gerenciamento |
| **CloudHSM** | FIPS 140-2 Level 3 / HSM dedicado / single-tenant |

⚠️ CloudHSM = alto overhead. Se pedir "menor overhead" → NÃO é CloudHSM.
</details>

<details>
<summary><strong>Gateway Endpoint vs Interface Endpoint — quando usar cada?</strong></summary>

- **Gateway Endpoint** (gratuito): SOMENTE S3 e DynamoDB. Configurado na route table.
- **Interface Endpoint** (pago): Todos os outros serviços (SQS, SNS, KMS, CloudWatch...). Cria ENI na subnet.

Se a prova oferecer "Gateway Endpoint para CloudWatch" → está ERRADO.
</details>

<details>
<summary><strong>WAF vs Shield — qual protege contra o quê?</strong></summary>

- **WAF** (Layer 7): SQL injection, XSS, bots, rate limiting, geo-blocking. Se associa ao ALB/CloudFront/API Gateway.
- **Shield Standard** (Layer 3/4): DDoS volumétrico. Gratuito, já ativo em toda conta.
- **Shield Advanced**: DDoS + suporte 24/7 DRT + proteção de custo. $3.000/mês.
</details>

<details>
<summary><strong>Cognito User Pool vs Identity Pool — qual faz o quê?</strong></summary>

- **User Pool:** Autentica usuários (cadastro, login, MFA). Retorna token JWT.
- **Identity Pool:** Troca token por credenciais AWS temporárias para acessar S3/DynamoDB diretamente.

"App mobile + login de clientes" → Cognito. "SSO para funcionários em múltiplas contas" → IAM Identity Center.
</details>

<details>
<summary><strong>O que é SCP e como se diferencia de IAM Policy?</strong></summary>

- **SCP** define o TETO de permissões para contas na organização. Mesmo admin não ultrapassa.
- SCP **não concede** permissões — só restringe.
- "Impedir mesmo com admin" / "nenhuma conta pode..." → SCP.
</details>

<details>
<summary><strong>AWS RAM serve para compartilhar S3 entre contas?</strong></summary>

**NÃO.** RAM compartilha infraestrutura (subnets, Transit Gateways, Route 53 rules). Para S3 cross-account → Bucket Policy ou Cross-account Role.
</details>

<details>
<summary><strong>CloudTrail vs Config vs GuardDuty — qual responde qual pergunta?</strong></summary>

| Serviço | Pergunta |
|---|---|
| **CloudTrail** | Quem fez o quê e quando? (auditoria) |
| **Config** | Meus recursos estão em conformidade? (compliance) |
| **GuardDuty** | Tem algo suspeito acontecendo? (ameaças com ML) |

GuardDuty detecta mas NÃO bloqueia. Para ação automática → EventBridge + Lambda.
</details>

<details>
<summary><strong>KMS vs Secrets Manager vs Parameter Store?</strong></summary>

- **KMS:** Chaves de criptografia (criptografar S3, EBS, RDS).
- **Secrets Manager:** Segredos com rotação automática (senhas RDS, API keys).
- **Parameter Store:** Configs simples e segredos sem rotação nativa.

"Rotação automática de senha" → Secrets Manager. "Criptografar dados" → KMS.
</details>

---

## Domínio 2 — Resiliência (26%)

<details>
<summary><strong>Quais são as 4 estratégias de Disaster Recovery (da mais barata à mais cara)?</strong></summary>

1. **Backup & Restore** ($) — RTO: horas. Nada rodando na DR.
2. **Pilot Light** ($$) — RTO: 15-30 min. Dados replicados + AMIs prontas, sem compute.
3. **Warm Standby** ($$$) — RTO: minutos. Infra mínima já rodando.
4. **Active-Active** ($$$$) — RTO: ~0. Infra completa em 2+ regiões.

Multi-AZ ≠ DR. Multi-AZ = falha de AZ. DR = falha de REGIÃO.
</details>

<details>
<summary><strong>RDS Multi-AZ vs Read Replica — diferença principal?</strong></summary>

- **Multi-AZ:** Alta disponibilidade. Replicação síncrona, failover automático, standby NÃO aceita queries.
- **Read Replica:** Performance de leitura. Replicação assíncrona, promoção manual, aceita queries SELECT.

"Usar standby para leitura" → SEMPRE ERRADO. São complementares (pode ter os dois).
</details>

<details>
<summary><strong>Banco sofrendo com muitas escritas no pico. Read Replica resolve?</strong></summary>

**NÃO.** Read Replica é somente leitura. Para problema de escrita → **SQS** para desacoplar e absorver picos (queue-based decoupling).

Muita leitura → Read Replica. Muita escrita → SQS como buffer.
</details>

<details>
<summary><strong>Padrão Fan-out: por que SNS + SQS e não só SNS?</strong></summary>

SNS puro: se o destino estiver fora do ar, perde a mensagem. SNS + SQS: mensagem fica na fila esperando o serviço voltar.

SQS atua como buffer durável. "Notificar múltiplos serviços + garantia de entrega" → SNS + múltiplas SQS.
</details>

<details>
<summary><strong>Kinesis Data Streams vs SQS — quando usar cada?</strong></summary>

- **SQS:** Desacoplamento, mensagem consumida e deletada, 1 consumer por mensagem.
- **Kinesis:** Streaming tempo real, múltiplos consumers, reprocessamento, retenção 1-365 dias.

"Múltiplos consumers + reprocessar + streaming" → Kinesis. "Buffer + desacoplar" → SQS.
</details>

<details>
<summary><strong>SQS Standard vs SQS FIFO?</strong></summary>

- **Standard:** Throughput ilimitado, ordem não garantida, possíveis duplicatas.
- **FIFO:** Ordem garantida, exactly-once, máx 300 msg/s (3.000 com batching).

"Ordem exata" ou "processar na sequência" → FIFO.
</details>

<details>
<summary><strong>Kinesis Firehose entrega para RDS?</strong></summary>

**NÃO.** Firehose entrega para: S3, Redshift, OpenSearch, Splunk. Só esses.
</details>

---

## Domínio 3 — Alta Performance (24%)

<details>
<summary><strong>CloudFront vs Global Accelerator — quando usar cada?</strong></summary>

- **CloudFront:** Conteúdo estático/cacheável (HTTP/HTTPS). CDN.
- **Global Accelerator:** Apps dinâmicas, TCP/UDP, IPs anycast estáticos, failover entre regiões.

Se NÃO é HTTP/HTTPS → Global Accelerator é a única opção. DNS (UDP) entre regiões → Global Accelerator.
</details>

<details>
<summary><strong>ALB vs NLB vs GWLB — quando usar cada?</strong></summary>

- **ALB** (Layer 7): HTTP/HTTPS, roteamento por path/host, microserviços, WAF.
- **NLB** (Layer 4): TCP/UDP, latência ultrabaixa, IP estático, gaming.
- **GWLB** (Layer 3): Appliances de segurança, firewall, IDS/IPS.

ALB NÃO tem IP estático. NLB NÃO entende HTTP.
</details>

<details>
<summary><strong>ElastiCache — quando usar e quando NÃO usar?</strong></summary>

✅ **Usar:** Queries repetitivas e idênticas acessadas milhares de vezes (session store, leaderboard, catálogo).

❌ **NÃO usar:** Relatórios de analytics com parâmetros variáveis → Read Replica resolve melhor.
</details>

<details>
<summary><strong>Transfer Acceleration é para download ou upload?</strong></summary>

**Upload** (usuário → S3). Usa edge locations para acelerar uploads para S3 via backbone AWS.

"Vídeo demora para carregar (download)" → CloudFront.
"Arquivo demora para subir (upload)" → Transfer Acceleration.
</details>

<details>
<summary><strong>Lambda@Edge — quando usar em vez de CloudFront com cache?</strong></summary>

Se dois usuários acessarem a mesma URL e devem receber respostas DIFERENTES (personalizado, saldo, perfil) → **Lambda@Edge** (compute no edge, sem cache).

Se devem receber a MESMA resposta → **CloudFront com cache**.
</details>

---

## Domínio 4 — Custo (20%)

<details>
<summary><strong>S3 Glacier Instant vs Flexible vs Deep Archive?</strong></summary>

- **Glacier Instant:** Acesso raro, recuperação em milissegundos. Compliance com acesso imediato.
- **Glacier Flexible:** Recuperação de minutos a horas.
- **Deep Archive:** Menor custo, recuperação até 12 horas. Retenção regulatória 7-10 anos.
</details>

<details>
<summary><strong>S3 Intelligent-Tiering — quando usar?</strong></summary>

Quando o **padrão de acesso é desconhecido ou imprevisível**. Move objetos automaticamente entre tiers. Cobra taxa de monitoramento por objeto.
</details>

<details>
<summary><strong>Spot Instances — quando são a resposta certa?</strong></summary>

Quando o cenário mencionar: "pode ser interrompido", "tolerante a falhas", "checkpoints", "batch", "rendering", "ML training", "maior redução de custo".

Desconto até 90%. Aviso de 2 minutos antes da interrupção.
</details>

<details>
<summary><strong>Reserved Instances vs Savings Plans — qual é mais flexível?</strong></summary>

**Compute Savings Plans** = mais flexível (qualquer tipo EC2 + Fargate + Lambda, qualquer região).

RI = tipo e região fixos. Na prova, "flexibilidade + desconto + compromisso" → Compute Savings Plans.
</details>

<details>
<summary><strong>S3 Object Lock: modo Governance vs Compliance?</strong></summary>

- **Governance:** Usuários com permissão especial podem deletar antes do prazo.
- **Compliance:** NINGUÉM pode deletar, nem o root da conta.

Regulatório + LGPD + HIPAA → Object Lock modo Compliance.
</details>

<details>
<summary><strong>Workload roda 3h/dia e tolera interrupção. Reserved Instance é boa opção?</strong></summary>

**NÃO.** RI cobra por hora contínua (24/7). Nesse caso → **Spot** (menor custo + tolera interrupção). RI/Savings Plans só compensam para uso 24/7 previsível.
</details>

---

## 🎯 Bônus — Erros de prova

<details>
<summary><strong>Serviço DNS multi-região com NLBs. CloudFront resolve a otimização de latência?</strong></summary>

**NÃO.** DNS usa UDP porta 53. CloudFront só suporta HTTP/HTTPS. A resposta correta é **AWS Global Accelerator** com NLBs como endpoints — suporta TCP/UDP e roteia pela latência real via IPs anycast.
</details>

<details>
<summary><strong>RDS Proxy distribui queries para o standby Multi-AZ?</strong></summary>

**NÃO.** RDS Proxy faz connection pooling (reutiliza conexões). NÃO transforma standby em leitor. Para offload de leitura → Read Replica.
</details>

<details>
<summary><strong>EventBridge garante entrega se o destino estiver fora do ar?</strong></summary>

**NÃO.** EventBridge não tem fila de buffer. Para garantia de entrega → SQS. Padrão: SNS → SQS → serviço.
</details>

---

---

## 🔥 Erros do Simulado — Tópicos Novos

<details>
<summary><strong>AWS Glue está reprocessando todos os dados a cada execução. Como resolver?</strong></summary>

**Job Bookmarks.** Rastreiam quais dados já foram processados. Na próxima execução, só processa dados novos/modificados.

Não é: deletar dados (perde integridade), FindMatches (é ML para deduplicação), NumberOfWorkers (só altera performance).
</details>

<details>
<summary><strong>Amazon MQ: quando usar em vez de SQS?</strong></summary>

Quando a aplicação **já usa protocolos de mensageria** (ActiveMQ, RabbitMQ, AMQP, MQTT, STOMP) — tipicamente migração de sistema legado.

Para apps cloud-native novas → SQS. "Sistema migrado" + "mensageria existente" → Amazon MQ.
</details>

<details>
<summary><strong>HA com menor complexidade: Auto Scaling Group ou EC2 extra em outra AZ?</strong></summary>

Se o foco é **menor complexidade** (não escalabilidade), uma instância EC2 extra em outra AZ pode ser suficiente. ASG adiciona configs de scaling policies, health checks customizados, etc.

"HA + menor complexidade + banco" → RDS Multi-AZ (não MySQL em EC2 com replicação manual).
</details>

<details>
<summary><strong>CloudWatch Logs → OpenSearch com menor esforço. Lambda?</strong></summary>

**NÃO.** Lambda adiciona código para manter. Usar:
- **Subscription Filter** (envio direto, sem código)
- **Kinesis Firehose** (se precisar de transformação/buffering)

"Menor esforço" = serviços gerenciados, nunca agentes manuais ou Lambda custom.
</details>

<details>
<summary><strong>Copiar arquivos entre buckets S3 automaticamente. Usar Lambda?</strong></summary>

**NÃO.** Usar **S3 Replication** (nativo, sem código, gerenciado). Lambda para copiar = overhead desnecessário.

Para notificar múltiplos serviços após o objeto chegar → **EventBridge** (não S3 Event Notification, que é limitada a 1 destino por tipo).
</details>

<details>
<summary><strong>10 TB alto I/O + 300 TB acesso frequente + 900 TB arquivo raro. Quais serviços?</strong></summary>

- **EBS** (io2/gp3): alto I/O, baixa latência, processamento intensivo
- **S3**: volume grande, acesso frequente, escalável
- **S3 Glacier**: arquivo raramente acessado, custo mínimo

⚠️ Instance Store = efêmero (dados perdidos). EFS = caro para 300 TB vs S3.
</details>

<details>
<summary><strong>ALB pode ficar em subnet privada?</strong></summary>

**NÃO** (para tráfego da internet). ALB precisa de subnet **pública** para receber requisições externas. EC2 e RDS ficam em subnets privadas. EC2 acessa internet via NAT Gateway.
</details>

<details>
<summary><strong>Spot + EKS vs Spot + ASG: qual tem menor complexidade?</strong></summary>

**Spot + Auto Scaling Group.** EKS adiciona Kubernetes (camada extra de complexidade). Se a questão pede "menor custo + menor overhead" e containers toleram interrupção → EC2 Spot + ASG.
</details>

<details>
<summary><strong>Site com conteúdo estático (S3) e dinâmico (ALB). Precisa de Global Accelerator?</strong></summary>

**NÃO.** CloudFront com **múltiplas origens** (S3 + ALB) resolve tudo em uma distribuição. Route 53 aponta para o CloudFront. Global Accelerator é para TCP/UDP não-HTTP.
</details>

<details>
<summary><strong>S3 Storage Lens: para que serve?</strong></summary>

Gera **métricas e relatórios** sobre uso de S3 em toda a organização (multi-conta, multi-região). Mostra: uploads multipart incompletos, tendências de crescimento, objetos não acessados.

Não é: Multi-Region Access Point (acesso, não relatórios), AWS Config (conformidade de config), SCP (restrição).
</details>

<details>
<summary><strong>"Ninguém pode deletar por 1 ano" — Governance ou Compliance?</strong></summary>

**Compliance.** Governance permite override com permissão especial. Compliance = NINGUÉM, nem root, nem AWS.

"Ninguém pode" = Compliance. "Admin pode override" = Governance.
</details>

<details>
<summary><strong>Impedir criação de IGW + restringir região. Quais serviços combinados?</strong></summary>

1. **SCP** (prevenção): impede criação de IGW e uso de regiões proibidas
2. **AWS Config** (detecção): alerta se recurso proibido for criado
3. **VPC sem IGW + SG + NACL** (rede): bloqueio físico de tráfego

Route 53, WAF e Control Tower NÃO resolvem esse cenário.
</details>

---

> 📅 Última atualização: Julho 2026
