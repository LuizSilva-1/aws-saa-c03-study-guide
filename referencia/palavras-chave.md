# Palavras-chave → Serviço (Tabela Mestre)

> Use esta tabela para revisão rápida antes da prova. Quando identificar uma palavra-chave no cenário, ela aponta diretamente para o serviço correto.

---

## Tabela Mestre

| Palavra-chave no cenário | Serviço/Solução |
|---|---|
| "< 10ms" / "single-digit milliseconds" | DynamoDB |
| "serverless + escala massiva" | Lambda + DynamoDB |
| "carga imprevisível + SQL" | Aurora Serverless |
| "compartilhar arquivos Linux" | EFS |
| "SMB + Active Directory" | FSx for Windows |
| "HPC + ML + arquivos" | FSx for Lustre |
| "fan-out + garantia de entrega" | SNS + múltiplas SQS |
| "conteúdo estático + cache + CDN" | CloudFront |
| "IPs fixos + TCP/UDP + failover entre regiões" | Global Accelerator |
| "upload rápido para S3" | Transfer Acceleration |
| "compliance + nunca deletar" | S3 Object Lock Compliance |
| "rotação automática de segredo/senha" | Secrets Manager |
| "criptografar dados + controle sobre chaves" | KMS CMK |
| "sem access key na EC2" | IAM Role |
| "sem sair para internet" / "tráfego privado" | VPC Endpoint |
| "HA do banco" / "failover automático RDS" | RDS Multi-AZ |
| "offload de leitura" / "relatórios degradando" | Read Replica |
| "absorver picos de escrita" / "banco sufocando" | SQS + Lambda |
| "tolerante a interrupção + menor custo" | Spot Instances |
| "uso 24/7 por 1-3 anos" | Reserved / Savings Plans |
| "bloquear IP específico" | NACL |
| "nenhum bucket público na conta" | S3 Block Public Access |
| "on-premises + nuvem + arquivos" | Storage Gateway |
| "backup de fitas" | Tape Gateway |
| "S3 ou DynamoDB sem internet" | Gateway Endpoint (gratuito) |
| "outros serviços AWS sem internet" | Interface Endpoint (pago) |
| "roteamento por path/host HTTP" | ALB |
| "latência ultrabaixa + IP estático + TCP" | NLB |
| "inspecionar tráfego + firewall" | GWLB |
| "site de notícias + picos + menor custo" | S3 + CloudFront |
| "múltiplas EC2 mesmo disco" | EFS (não EBS) |
| "Firehose destinos" | S3, Redshift, OpenSearch (NÃO RDS) |
| "padrão de acesso desconhecido S3" | Intelligent-Tiering |
| "menor custo armazenamento + posso esperar 12h" | Glacier Deep Archive |
| "SQL injection, XSS, bots" | WAF |
| "DDoS" (sem extras) | Shield Standard (gratuito) |
| "DDoS + suporte 24/7" | Shield Advanced |
| "quem fez o quê, auditoria" | CloudTrail |
| "conformidade, compliance, regras" | AWS Config |
| "notificar quando violar regra de conformidade" | AWS Config + SNS |
| "corrigir automaticamente recurso fora da regra" | AWS Config + EventBridge + Lambda |
| "comportamento anômalo, ameaça" | GuardDuty |
| "GuardDuty detectou + ação automática" | GuardDuty + EventBridge + Lambda |
| "impedir ação em toda a organização" | SCP |
| "gerenciar múltiplas contas" | AWS Organizations |
| "compartilhar subnet entre contas" | AWS RAM |
| "conta A acessar S3 da conta B" | Bucket Policy ou Cross-account Role |
| "conectar múltiplas VPCs" | Transit Gateway |
| "rejeitar upload sem criptografia no S3" | Bucket Policy (Deny PutObject) |
| "forçar HTTPS no S3" | Bucket Policy (SecureTransport) |
| "streaming + múltiplos consumers + retenção" | Kinesis Data Streams |
| "entregar dados para S3/Redshift sem código" | Kinesis Firehose |
| "DR + RTO horas + menor custo" | Backup & Restore |
| "DR + RTO 30 min + custo moderado" | Pilot Light ou Warm Standby |
| "DR + zero downtime" | Multi-Site Active-Active |
| "personalizado + latência < 100ms global" | Lambda@Edge |
| "flexibilidade tipo instância + desconto + EC2/Fargate/Lambda" | Compute Savings Plans |
| "acesso temporário a arquivo S3 sem tornar público" | Presigned URL (máx 7 dias) |
| "app mobile/web + autenticar usuários clientes" | Cognito User Pool |
| "usuário do app acessa S3/DynamoDB diretamente" | Cognito Identity Pool |
| "login social (Google/Facebook) na aplicação" | Cognito User Pool |
| "login único para funcionários em N contas AWS" | IAM Identity Center |
| "SSO + Active Directory corporativo + acesso AWS" | IAM Identity Center |
| "landing zone, setup multi-conta do zero" | Control Tower |
| "certificado SSL/TLS para ALB/CloudFront" | ACM |
| "dados sensíveis no S3 / PII / LGPD / cartão de crédito" | Macie |
| "vulnerabilidade em EC2 / CVE / patch / container" | Inspector |
| "múltiplas condições para disparar alarme" | CloudWatch Composite Alarm |
| "reduzir falsos alarmes" | CloudWatch Composite Alarm |
| "ETL reprocessando dados antigos no Glue" | Glue Job Bookmarks |
| "consultar dados no S3 sem servidor" | Amazon Athena |
| "queries ad-hoc em data lake" | Amazon Athena |
| "RabbitMQ gerenciado na AWS" | Amazon MQ |
| "migrar fila RabbitMQ/ActiveMQ" | Amazon MQ |
| "broker ativo/standby multi-AZ" | Amazon MQ |
| "acesso ao SO + banco gerenciado" | RDS Custom |
| "Oracle com personalização avançada" | RDS Custom for Oracle |
| "RPO baixo + DynamoDB" | DynamoDB PITR |
| "auditoria de mudanças DynamoDB" | DynamoDB Streams |
| "copiar objetos entre buckets automaticamente" | S3 Replication (não Lambda) |
| "múltiplos destinos após chegada no S3 (Lambda + SageMaker)" | S3 Replication + EventBridge |
| "mesma chave KMS em múltiplas regiões" | KMS multi-region key |
| "criptografar + replicar cross-region + mesma chave" | SSE-KMS multi-region + CRR |
| "logs CloudWatch → OpenSearch, tempo real" | Subscription Filter ou Kinesis Firehose |
| "centenas de TB + prazo curto + banda limitada" | AWS Snowball |
| "armazenar por anos + acesso raramente + menor custo" | S3 Glacier Deep Archive |
| "mover dados entre classes S3 automaticamente" | S3 Lifecycle Policy |
| "ALB em qual subnet?" | Sempre subnet **pública** |
| "EC2 / RDS em qual subnet?" | Sempre subnet **privada** |
| "EC2 privada acessa internet para updates" | NAT Gateway (na subnet pública) |
| "gateway de internet privado" | Não existe — sempre é NAT Gateway |
| "Fargate + Lambda + desconto comprometido" | Compute Savings Plan (não EC2 SP) |
| "EC2 mínimo garantido + picos" | Reserved (mínimo) + Spot (picos) |
| "Static site + atualizações esporádicas" | S3 static hosting + CloudFront |
| "CloudFront fornece HTTPS?" | Sim, via ACM — WAF não fornece HTTPS |
| "monitorar endpoint / simular usuário" | CloudWatch Synthetics (canário) |

---

*Referência rápida para a prova SAA-C03*
