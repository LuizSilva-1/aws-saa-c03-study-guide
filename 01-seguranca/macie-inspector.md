# Macie e Inspector

## A distinção principal — mais cobrada na prova

| Serviço | O que detecta | Onde atua |
|---|---|---|
| **Macie** | Dados sensíveis (PII, CPF, cartão de crédito, senhas) | Somente **S3** |
| **Inspector** | Vulnerabilidades de software (CVEs, portas abertas, patches) | **EC2**, containers (ECS/EKS), Lambda |

> Regra direta: "dados sensíveis no S3 / PII / LGPD / cartão de crédito" → **Macie**. "Vulnerabilidade em instância / CVE / patch / porta exposta" → **Inspector**.

---

## Amazon Macie

**O que faz:** Usa ML para descobrir e classificar dados sensíveis em buckets S3 automaticamente.

**Exemplos do que detecta:**
- CPF, RG, passaporte
- Números de cartão de crédito
- Credenciais de acesso (access keys expostas)
- Informações de saúde (PHI)
- Qualquer dado que se enquadre em LGPD, HIPAA, PCI-DSS

**Fluxo:**
```
Macie escaneia buckets S3
    ↓
Identifica objeto com dados sensíveis
    ↓
Gera finding (alerta)
    ↓
EventBridge + SNS notifica o time
```

**Na prova:** "PII", "dados sensíveis", "cartão de crédito no S3", "LGPD", "compliance de dados"

---

## Amazon Inspector

**O que faz:** Escaneia EC2, containers e Lambda em busca de vulnerabilidades de software e configurações inseguras.

**Exemplos do que detecta:**
- CVEs (vulnerabilidades conhecidas em pacotes instalados)
- Porta 22 (SSH) aberta para 0.0.0.0/0
- Software desatualizado sem patch de segurança
- Configurações inseguras em containers

**Importante:** Inspector é **contínuo e automático** — não precisa agendar scans manualmente. Assim que uma nova CVE é publicada, ele reavalia automaticamente.

**Na prova:** "vulnerabilidade", "CVE", "patch", "porta aberta na instância", "segurança de container"

---

## Erro comum — não confundir com GuardDuty

| Serviço | Foco |
|---|---|
| **Inspector** | Vulnerabilidades na **infraestrutura** (EC2, containers) |
| **GuardDuty** | Comportamento **anômalo em tempo real** (atividade maliciosa) |
| **Macie** | Dados **sensíveis em S3** |

Três serviços de detecção, cada um com escopo diferente. Nunca são intercambiáveis na prova.

---

## Presigned URL (S3)

> Incluído aqui por ser outro mecanismo de controle de acesso a dados no S3.

**O que é:** URL temporária que dá acesso a um objeto S3 específico sem tornar o bucket público.

**Características:**
- Válida por tempo configurável — máximo **7 dias**
- Pode ser gerada via CLI ou SDK (não apenas CLI)
- Quem gera precisa ter permissão no objeto — se a permissão for revogada, a URL para de funcionar mesmo dentro do prazo
- Acesso é para **um único objeto** específico

**Na prova:** "acesso temporário a arquivo S3 para usuário externo sem tornar público" → **Presigned URL**

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
