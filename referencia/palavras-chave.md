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

---

*Referência rápida para a prova SAA-C03*
