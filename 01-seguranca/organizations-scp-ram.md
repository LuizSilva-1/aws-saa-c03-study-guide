# AWS Organizations, SCPs e RAM

## AWS Organizations

**O que faz:** Gerencia múltiplas contas AWS de forma centralizada.

**Funcionalidades:**
- Faturamento consolidado (uma fatura para todas as contas)
- Criar contas programaticamente
- Organizar contas em OUs (Organizational Units)
- Aplicar SCPs (Service Control Policies)

**Na prova:** "múltiplas contas", "faturamento unificado/consolidado", "governança centralizada"

---

## Organizations vs Control Tower

| | Organizations | Control Tower |
|---|---|---|
| **O que é** | Serviço base para multi-conta | Camada de automação em cima do Organizations |
| **Quando usar** | Gerenciar contas existentes | Configurar ambiente multi-conta do ZERO |
| **Na prova** | "gerenciar contas", "faturamento" | "landing zone", "setup com boas práticas" |

> Control Tower usa Organizations por baixo. Na prova, se não mencionar "landing zone" ou "setup inicial" → Organizations.

---

## SCP (Service Control Policies)

**O que faz:** Define o TETO de permissões para contas na organização. Mesmo um admin na conta não pode ultrapassar o SCP.

**Importante:**
- SCP **não concede** permissões — ele só **restringe**
- O user precisa ter permissão via IAM Policy E o SCP precisa permitir
- Se SCP nega → nada funciona, independente das policies do user

**Exemplos comuns:**
- Impedir criação de recursos fora de regiões permitidas
- Impedir desabilitação do CloudTrail
- Impedir saída da organização

**Na prova:** "impedir mesmo com admin", "restringir toda a organização", "nenhuma conta pode..."

---

## AWS RAM (Resource Access Manager)

**O que faz:** Compartilha INFRAESTRUTURA AWS entre contas da mesma organização SEM duplicar.

**O que pode compartilhar (recursos de infraestrutura):**
- Subnets (contas lançam EC2 na mesma subnet diretamente)
- Transit Gateways (conta central cria, outras se conectam)
- Route 53 Resolver rules
- License Manager configs
- Aurora DB clusters

**O que RAM NÃO faz:**
- ❌ Não dá acesso a S3 de outra conta (usar Bucket Policy ou Cross-account Role)
- ❌ Não dá acesso a DynamoDB de outra conta (usar Cross-account Role)
- ❌ Não é um endpoint (Endpoint = rede interna. RAM = compartilhar infraestrutura)

**Analogia:** RAM = duas empresas usando o mesmo prédio comercial. Cross-account Role = uma empresa dando a chave do escritório pra outra.

**Na prova:** "compartilhar subnet entre contas", "usar mesma infra em múltiplas contas sem peering"

---

## Cross-account access (EC2 da conta A acessa S3 da conta B)

RAM NÃO resolve isso. As soluções são:

**Opção 1 — Bucket Policy (resource-based, mais simples):**
- Bucket na conta B tem policy que permite o ARN da Role da conta A
- EC2 na conta A acessa direto

**Opção 2 — Cross-account Role (identity-based, mais segura):**
- Conta B cria Role com permissão no S3
- EC2 na conta A faz `AssumeRole` → recebe credenciais temporárias
- Usa essas credenciais para acessar S3 da conta B

**Na prova:**

| Cenário | Solução |
|---|---|
| "Conta A acessar bucket da conta B" | **Bucket Policy** ou **Cross-account Role** |
| "Compartilhar subnet entre contas" | **AWS RAM** |
| "Conectar VPCs de contas diferentes" | **Transit Gateway** ou **VPC Peering** |

---

## Transit Gateway vs VPC Peering vs RAM

| | VPC Peering | Transit Gateway | AWS RAM |
|---|---|---|---|
| **O que faz** | Conecta 2 VPCs (rede) | Hub central para múltiplas VPCs | Compartilha recursos entre contas |
| **Escala** | 1:1 (cresce exponencialmente) | Hub-and-spoke (escala bem) | N/A |
| **Quando usar** | 2-3 VPCs | Muitas VPCs + on-premises | Compartilhar subnet específica |
| **Peering necessário?** | Sim | Não (hub central) | Não |

---

## Palavras-chave na prova

| Cenário | Serviço |
|---|---|
| "Gerenciar múltiplas contas" | **Organizations** |
| "Faturamento consolidado" | **Organizations** |
| "Impedir ação em todas as contas" | **SCP** |
| "Nem admin pode fazer X" | **SCP** |
| "Compartilhar subnet entre contas" | **AWS RAM** |
| "Conectar múltiplas VPCs" | **Transit Gateway** |
| "Conectar 2 VPCs" | **VPC Peering** |
| "Conta A acessar S3 da conta B" | **Bucket Policy** ou **Cross-account Role** |
| "Landing zone", "setup multi-conta" | **Control Tower** |

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
