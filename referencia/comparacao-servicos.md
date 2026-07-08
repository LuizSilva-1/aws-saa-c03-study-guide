# Comparação de Serviços AWS

## Computação

| Serviço | Use quando... |
|---|---|
| **EC2** | Controle total do SO, workloads legados, licenciamento |
| **Lambda** | Eventos, execuções < 15 min, escala para zero |
| **Fargate** | Containers sem gerenciar EC2, paga pelo uso |
| **ECS** | Containers com integração AWS nativa |
| **EKS** | Kubernetes, portabilidade multi-cloud |

---

## Banco de dados

| Serviço | Tipo | Use quando... |
|---|---|---|
| **RDS** | Relacional | SQL padrão (MySQL, PostgreSQL), OLTP |
| **Aurora** | Relacional | Alta performance (5x MySQL), HA automático |
| **Aurora Serverless** | Relacional | Carga imprevisível + SQL + custo variável |
| **DynamoDB** | NoSQL | Latência < 10ms, escala massiva, serverless |
| **Redshift** | Data Warehouse | OLAP, analytics em grandes volumes históricos |
| **ElastiCache** | Cache in-memory | Reduzir latência, sessões, queries repetitivas |

> **"single-digit milliseconds"** + escala massiva = DynamoDB, sempre. Elimina qualquer relacional.

---

## Armazenamento

| Serviço | Protocolo | Use quando... |
|---|---|---|
| **S3** | Object/HTTP | Arquivos, data lakes, backups, websites estáticos |
| **EBS** | Block | Disco de UMA EC2 (não compartilha) |
| **EFS** | NFS (Linux) | Compartilhar entre múltiplas EC2 Linux |
| **FSx for Windows** | SMB | Windows + Active Directory |
| **FSx for Lustre** | Lustre | HPC, ML, integra com S3 |

> **Múltiplas EC2 compartilhando** → EFS (Linux) ou FSx (Windows). NÃO EBS.

---

## Mensageria e Streaming

| Serviço | Padrão | Use quando... |
|---|---|---|
| **SQS** | Fila (pull) | Desacoplar, buffer, garantir processamento |
| **SNS** | Pub/Sub (push) | Notificar múltiplos destinos |
| **EventBridge** | Event bus | Orquestrar entre serviços, regras complexas |
| **Kinesis Data Streams** | Streaming | Ingestão em tempo real, múltiplos consumers |
| **Kinesis Firehose** | Delivery | Entregar para S3, Redshift, OpenSearch |

> **Firehose NÃO grava em RDS.** Destinos: S3, Redshift, OpenSearch, Splunk.

---

## Storage Gateway (Híbrido)

| Modalidade | O que faz |
|---|---|
| **File Gateway** | Arquivos on-premises via NFS/SMB → S3 |
| **Volume Gateway** | Volumes iSCSI on-premises → backup na AWS |
| **Tape Gateway** | Fitas virtuais → S3/Glacier |
| **FSx File Gateway** | Cache local de FSx for Windows |

---

## Networking

| Serviço | Use quando... |
|---|---|
| **CloudFront** | Entrega de conteúdo (download), cache global |
| **Global Accelerator** | App dinâmica global, IPs fixos, TCP/UDP, failover |
| **Transfer Acceleration** | Upload rápido para S3 |
| **Route 53** | DNS, roteamento (geolocation, latency, failover) |
| **VPN** | Conexão criptografada on-premises ↔ AWS |
| **Direct Connect** | Conexão dedicada on-premises ↔ AWS (alta banda) |
| **Transit Gateway** | Hub central para conectar múltiplas VPCs e on-premises |

---

*Referência rápida para a prova SAA-C03*
