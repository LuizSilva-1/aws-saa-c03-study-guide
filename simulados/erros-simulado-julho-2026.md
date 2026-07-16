# Erros do Simulado — Julho 2026

> Notas de estudo baseadas nas questões erradas. Foco nos tópicos novos que não tinham cobertura.

---

## 1. AWS Glue Job Bookmarks (Questão 8)

### O que é

Recurso do AWS Glue que **rastreia quais dados já foram processados**. Na próxima execução, o job só processa dados novos ou modificados.

### Quando usar

| Cenário | Solução |
|---|---|
| "ETL reprocessando todos os dados a cada execução" | **Job Bookmarks** |
| "Processar apenas dados novos no S3" | **Job Bookmarks** |
| "Processamento incremental" | **Job Bookmarks** |

### O que NÃO resolve o problema

| Opção errada | Por quê |
|---|---|
| Deletar dados do S3 após processar | Perde dados, compromete auditoria |
| FindMatches (ML) | Deduplicação de registros, não controla incremento |
| NumberOfWorkers = 1 | Apenas altera paralelismo/performance |

### Regra rápida

> AWS Glue + "evitar reprocessamento" + "dados novos" → **Job Bookmarks**

---

## 2. Amazon MQ — Alta Disponibilidade (Questão 16)

### O que é Amazon MQ

Serviço **gerenciado** de mensageria compatível com protocolos abertos (AMQP, MQTT, STOMP). Usado quando a aplicação já usa ActiveMQ ou RabbitMQ (migração de sistema legado).

### Amazon MQ vs Amazon SQS

| | Amazon MQ | Amazon SQS |
|---|---|---|
| **Quando usar** | Migração de sistema legado com ActiveMQ/RabbitMQ | Aplicação nova, cloud-native |
| **Protocolos** | AMQP, MQTT, STOMP, OpenWire | API AWS proprietária |
| **HA** | Ativo/standby Multi-AZ | Nativo (sem config) |
| **Na prova** | "migrou para AWS", "sistema existente de mensagens" | "desacoplar", "fila", "novo sistema" |

### Padrão de HA com menor complexidade

```
Amazon MQ (ativo/standby Multi-AZ)
    ↓
EC2 consumers em múltiplas AZs (instância extra, NÃO Auto Scaling)
    ↓
Amazon RDS Multi-AZ (failover automático gerenciado)
```

### Decisão na prova: complexidade operacional

| Componente | Menor complexidade | Maior complexidade |
|---|---|---|
| Banco de dados | **RDS Multi-AZ** (gerenciado) | MySQL em EC2 + replicação manual |
| Consumers EC2 | EC2 em outra AZ (simples) | Auto Scaling Group (mais configs) |
| Mensageria | Amazon MQ ativo/standby | MQ manual em 2 instâncias |

### ⚠️ Armadilha

Auto Scaling Group ≠ sempre a melhor resposta para HA. Se o foco é **menor complexidade** (não escalabilidade), uma instância extra em outra AZ pode ser suficiente.

> **Regra:** "HA + menor complexidade + banco" → RDS Multi-AZ. "HA + menor complexidade + MQ" → Amazon MQ ativo/standby.

---

## 3. CloudWatch Logs → OpenSearch (Questão 19)

### Formas de enviar logs do CloudWatch para OpenSearch

| Solução | Complexidade | Quando usar |
|---|---|---|
| **Subscription Filter direto** | Baixa | Envio simples, sem transformação |
| **Kinesis Data Firehose** | Baixa | Precisa de transformação/buffering |
| Lambda customizada | Alta | Evitar — overhead operacional |
| Kinesis Agent + Data Streams | Alta | Evitar — instalação manual em servidores |

### Palavras-chave na prova

| Cenário | Solução |
|---|---|
| "CloudWatch Logs → OpenSearch + menor esforço" | **Subscription Filter** ou **Firehose** |
| "tempo quase real + sem código" | **Subscription Filter** |
| "tempo quase real + transformação de dados" | **Firehose** (pode usar Lambda para transformar) |
| "instalar agente em servidores" | ❌ Complexidade alta, resposta errada |

### Regra rápida

> "Menor esforço operacional" + "logs → destino" → Serviços **gerenciados** (Subscription Filter, Firehose). NUNCA Lambda manual ou agentes.

---

## 4. S3 Replication + EventBridge (Questão 36)

### Padrão: mover arquivos entre buckets + notificar múltiplos serviços

```
Bucket de entrada
    ↓ (S3 Replication — automático, sem código)
Bucket de análise
    ↓ (EventBridge captura ObjectCreated)
    ├→ Lambda (processamento)
    └→ SageMaker Pipelines (ML)
```

### Por que EventBridge e não S3 Event Notification direta?

| | S3 Event Notification | EventBridge |
|---|---|---|
| Múltiplos destinos | Limitado (1 destino por tipo de evento) | ✅ Ilimitado (múltiplas regras) |
| Flexibilidade de filtro | Básica (prefixo/sufixo) | Avançada (JSON pattern matching) |
| Integração | SNS, SQS, Lambda | Centenas de serviços AWS |
| Na prova | Caso simples (1 destino) | "Múltiplos destinos" → EventBridge |

### Decisão na prova

| Cenário | Solução |
|---|---|
| "Copiar arquivos entre buckets" | **S3 Replication** (não Lambda) |
| "Múltiplos destinos ao criar objeto" | **EventBridge** (não S3 notification) |
| "Lambda para copiar arquivo" | ❌ Overhead desnecessário |

> **Regra:** Se a AWS já tem um serviço gerenciado que faz a tarefa → use-o. Lambda para copiar S3 = overhead.

---

## 5. Armazenamento por Perfil de Uso (Questão 49)

### Mapa de decisão de armazenamento

| Requisito | Serviço | Por quê |
|---|---|---|
| **Alto I/O + baixa latência** (processamento de vídeo) | **Amazon EBS** (io2/gp3) | Armazenamento em bloco, acoplado ao EC2 |
| **Grande volume + acesso frequente** (300 TB mídia ativa) | **Amazon S3** | Objetos, escalável, durável, custo-benefício |
| **Grande volume + raramente acessado** (900 TB arquivo) | **S3 Glacier** | Arquivamento econômico |

### Instance Store vs EBS — armadilha

| | Instance Store | EBS |
|---|---|---|
| **Performance** | Altíssima (SSD local NVMe) | Alta (io2 Block Express) |
| **Durabilidade** | ❌ EFÊMERO (dados perdidos ao parar/terminar EC2) | ✅ Persistente |
| **Na prova** | "temporário", "cache local", "não precisa manter" | **Qualquer outro caso** |

> **⚠️ REGRA:** Se o cenário pede "armazenamento durável" ou não diz explicitamente que dados podem ser perdidos → **EBS**, nunca Instance Store.

### EFS vs S3 para 300 TB

| | EFS | S3 |
|---|---|---|
| Protocolo | NFS (file system compartilhado) | API HTTP (objetos) |
| Ideal para | Múltiplas instâncias acessando mesmos arquivos simultaneamente | Armazenamento massivo, acesso por API |
| 300 TB | Caro e menos escalável | ✅ Mais econômico e escalável |
| Na prova | "compartilhar arquivos entre EC2s" | "armazenamento durável em larga escala" |

---

## 6. VPC — ALB público + EC2/RDS privados + NAT (Questões 25 e 42)

### Arquitetura padrão de e-commerce HA

```
Internet
    ↓
ALB (subnet PÚBLICA — obrigatório para receber tráfego externo)
    ↓
EC2 via Auto Scaling (subnet PRIVADA — não exposta)
    ↓
RDS Multi-AZ (subnet PRIVADA — não exposta)

EC2 → NAT Gateway (subnet pública) → Internet (saída para APIs de pagamento)
```

### Regras fixas

| Componente | Onde fica | Por quê |
|---|---|---|
| **ALB** | Subnet PÚBLICA | Precisa receber tráfego da internet |
| **EC2 (app)** | Subnet PRIVADA | Não deve ser acessível diretamente |
| **RDS** | Subnet PRIVADA | Banco nunca na internet |
| **NAT Gateway** | Subnet PÚBLICA | Permite saída das privadas para internet |

### NAT Gateway — HA

- **1 NAT Gateway por AZ** = alta disponibilidade
- Se usar 1 NAT Gateway só → ponto único de falha
- NAT Gateway vs NAT Instance: Gateway = gerenciado, escalável. Instance = mais controle, mais trabalho.

### ⚠️ Armadilhas na prova

| Opção errada | Por quê |
|---|---|
| "ALB em subnet privada" | ALB não recebe tráfego externo se estiver em privada |
| "EC2 em subnet pública" | Expõe instâncias à internet (violação de segurança) |
| "1 subnet privada em 1 AZ" | Sem HA — ponto único de falha |
| "Internet Gateway em subnet privada" | Não existe esse conceito. IGW se conecta à VPC, não subnet |

---

## 7. Spot + ASG vs EKS — Menor Custo + Complexidade (Questão 28)

### Decisão: contêineres na AWS

| Solução | Custo | Complexidade operacional |
|---|---|---|
| **EC2 Spot + Auto Scaling** | Muito baixo | Baixa |
| EC2 Spot + EKS | Muito baixo | **Alta** (Kubernetes) |
| EC2 On-Demand + ASG | Alto | Baixa |
| EC2 On-Demand + EKS | Alto | Alta |

### Regra na prova

> "Menor custo" + "tolera interrupção" + "menor complexidade" → **Spot + Auto Scaling Group**

EKS adiciona camada de complexidade (Kubernetes). Se a questão pede "menor overhead operacional" junto com "menor custo", EKS é distrator.

### Quando EKS seria certo?

- "Kubernetes existente" / "orquestração de contêineres complexa"
- "Microserviços com service mesh"
- NÃO quando o foco é "simplicidade"

---

## 8. CloudFront com Múltiplas Origens (Questão 41)

### Padrão: conteúdo estático + dinâmico

```
Route 53
    ↓
CloudFront (1 distribuição, múltiplas origens)
    ├→ Origem 1: S3 (estático — imagens, CSS, JS)
    └→ Origem 2: ALB (dinâmico — APIs, páginas geradas)
```

### Behaviors no CloudFront

CloudFront usa **Cache Behaviors** para rotear por path:
- `/static/*` → S3
- `/api/*` → ALB
- `/*` (default) → ALB

### Por que NÃO usar Global Accelerator aqui?

| Cenário | Serviço |
|---|---|
| Conteúdo HTTP estático + dinâmico em CDN | **CloudFront** |
| TCP/UDP não-HTTP + latência global | **Global Accelerator** |
| S3 como origem | **CloudFront** (GA não cacheia) |

> **Regra:** Se tudo é HTTP/HTTPS (site web) → CloudFront resolve sozinho com múltiplas origens. Não precisa de Global Accelerator.

---

## 9. SCP + AWS Config + VPC sem IGW — Compliance (Questão 51)

### 3 camadas de controle para compliance rigoroso

| Camada | Serviço | O que faz |
|---|---|---|
| **Prevenção** | SCP (Organizations) | Impede criação de IGW e uso de regiões proibidas |
| **Detecção** | AWS Config | Detecta e alerta se alguém conseguir criar recurso proibido |
| **Rede** | VPC sem IGW + SG + NACL | Bloqueia tráfego de saída fisicamente |

### O que NÃO funciona para esse cenário

| Opção errada | Por quê |
|---|---|
| Route 53 para bloquear DNS | Route 53 não restringe regiões nem bloqueia internet |
| Control Tower | Facilita governança mas não bloqueia diretamente |
| AWS WAF | Protege apps web, não controla regiões/IGW |

### Palavras-chave na prova

| Cenário | Solução |
|---|---|
| "Impedir criação de recurso" (prevenção) | **SCP** |
| "Detectar recurso fora de conformidade" (detecção) | **AWS Config** |
| "Sem acesso à internet em VPC" | **Sem IGW + SG + NACL restritivos** |
| "Restringir a uma região específica" | **SCP** |

---

## 10. S3 Object Lock — Compliance vs Governance (Questão 54)

### Diferença crítica

| Modo | Quem pode deletar/modificar antes do prazo? |
|---|---|
| **Governance** | Usuários com permissão especial (`s3:BypassGovernanceRetention`) |
| **Compliance** | **NINGUÉM** — nem root, nem admin, nem AWS |

### Decisão na prova

| Cenário | Modo |
|---|---|
| "Nenhum usuário pode modificar/excluir" | **Compliance** |
| "Regulatório", "HIPAA", "LGPD", "retenção obrigatória" | **Compliance** |
| "Proteção contra exclusão acidental, mas admin pode override" | **Governance** |
| "Ensaio médico", "dados legais" | **Compliance** |

> **Regra:** Se o cenário diz "ninguém pode" → Compliance. Se diz "apenas admins podem" → Governance.

---

## 11. S3 Storage Lens (Questão 61)

### O que é

Ferramenta nativa do S3 que gera **métricas e relatórios** sobre uso de armazenamento em toda a organização (multi-conta, multi-região).

### O que mostra

- Objetos armazenados e bytes usados
- **Uploads multipart incompletos** ← questão da prova
- Tendências de crescimento
- Objetos não acessados
- Métricas por conta, região, bucket

### Quando usar

| Cenário | Solução |
|---|---|
| "Relatório sobre uso de S3 em múltiplas contas" | **S3 Storage Lens** |
| "Uploads multipart incompletos" | **S3 Storage Lens** |
| "Visibilidade centralizada de armazenamento" | **S3 Storage Lens** |
| "Otimização de custos de S3" | **S3 Storage Lens** |

### O que NÃO serve para isso

| Opção errada | Por quê |
|---|---|
| S3 Multi-Region Access Point | Acesso unificado, não gera relatórios |
| AWS Config | Conformidade de configurações, não métricas de uso |
| SCP | Restrição de permissões, não monitoramento |

### Dica complementar: Lifecycle para limpar uploads incompletos

```json
{
  "Rules": [{
    "ID": "AbortIncompleteMultipartUpload",
    "Status": "Enabled",
    "AbortIncompleteMultipartUpload": {
      "DaysAfterInitiation": 7
    }
  }]
}
```

> **Regra:** "Relatório/visibilidade sobre uso do S3" → Storage Lens. "Limpar uploads incompletos" → Lifecycle Rule.

---

## 📊 Resumo — Padrões de erro

| Padrão de erro | Lição |
|---|---|
| Escolher Lambda quando existe serviço gerenciado | AWS prefere serviços nativos (Replication, Firehose, Subscription Filter) |
| Confundir complexidade com funcionalidade | "Menor complexidade" ≠ "mais features". ASG simples > EKS |
| Misturar serviços (CloudFront + GA) | Cada serviço tem seu caso. Não combinar quando 1 resolve sozinho |
| Ignorar que ALB PRECISA de subnet pública | ALB sempre público. EC2/RDS sempre privado |
| Governance vs Compliance | "Ninguém pode" = Compliance. Sem exceção |

---

> 📅 Simulado realizado em Julho 2026 | Score: analisar e melhorar nos próximos
