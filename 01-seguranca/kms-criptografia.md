# KMS e Criptografia

## Tipos de chave no KMS

| Tipo | Controle | Rotação | Overhead | Quando usar |
|---|---|---|---|---|
| **Customer Managed Key (CMK)** | Total (policies, auditoria) | Automática anual (habilitar) | Baixo | "Controle sobre chaves" + "menor overhead" |
| **AWS Managed Key** (aws/s3, aws/rds) | Nenhum | Automática (AWS gerencia) | Zero | Zero gerenciamento |
| **CloudHSM** | Total + HSM dedicado | Manual | Alto | FIPS 140-2 Level 3, compliance extremo |

---

## Decisão na prova

| Requisito no cenário | Solução |
|---|---|
| "Controle total sobre as chaves" + "menor overhead" | **KMS CMK** |
| "Zero gerenciamento de chaves" | **AWS Managed Key** |
| "HSM dedicado" / "FIPS 140-2 Level 3" / "single-tenant" | **CloudHSM** |
| "Rotação automática de chaves" | **KMS CMK** (habilitar) ou AWS Managed (já tem) |

> **⚠️** CloudHSM = controle total MAS alto overhead (gerenciar cluster). Se pedir "menor overhead" → NÃO é CloudHSM.

---

## KMS vs Secrets Manager vs Parameter Store

| Serviço | O que gerencia | Rotação automática | Exemplo |
|---|---|---|---|
| **KMS** | Chaves de criptografia | De chave (anual) | Criptografar S3, EBS, RDS |
| **Secrets Manager** | Segredos (senhas, tokens) | ✅ Nativa | Senha do RDS, API keys |
| **Parameter Store** | Configs e segredos simples | ❌ Não nativa | Variáveis de ambiente, configs |

> **Regra:** "criptografar dados" → KMS | "rotação automática de senha" → Secrets Manager

---

## Criptografia em repouso vs em trânsito

| Tipo | O que protege | Como |
|---|---|---|
| **Em repouso** | Dados armazenados (S3, EBS, RDS) | KMS, SSE-S3, SSE-KMS |
| **Em trânsito** | Dados trafegando na rede | TLS/SSL, HTTPS, VPN |

---

## S3 — opções de criptografia server-side

| Opção | Chave gerenciada por | Quando usar |
|---|---|---|
| **SSE-S3** | AWS (automático) | Padrão, sem necessidade de controle |
| **SSE-KMS** | KMS (CMK ou AWS Managed) | Auditoria (CloudTrail), controle de acesso à chave |
| **SSE-C** | Cliente (você envia a chave) | Controle total, responsabilidade total |

---

## Forçar criptografia no S3

Para rejeitar uploads sem criptografia, use uma **Bucket Policy** com condição:
```json
{
  "Condition": {
    "StringNotEquals": {
      "s3:x-amz-server-side-encryption": "aws:kms"
    }
  }
}
```

Ou habilite **Default Encryption** no bucket (desde 2023, S3 criptografa tudo por padrão com SSE-S3).

---

## Auditoria de uso de chaves

- **AWS CloudTrail** registra toda chamada à API do KMS
- Mostra: quem usou a chave, quando, para qual operação
- "Registro de auditoria de uso de chaves" → **CloudTrail + KMS CMK**

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
