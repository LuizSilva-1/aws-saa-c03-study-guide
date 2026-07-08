# IAM — Roles, Policies e Users

## Conceitos fundamentais

| Conceito | O que é | Analogia |
|---|---|---|
| **IAM User** | Identidade permanente com credenciais fixas | Crachá de funcionário |
| **IAM Role** | Identidade temporária que pode ser assumida | Crachá de visitante (temporário) |
| **IAM Policy** | Documento JSON com permissões | Lista de portas que o crachá abre |
| **IAM Group** | Coleção de Users que compartilham policies | Departamento da empresa |

---

## Role vs User — quando usar cada um

| Cenário | Usar |
|---|---|
| EC2 precisa acessar S3 | **Role** (sem access key na instância) |
| Lambda precisa acessar DynamoDB | **Role** (execution role) |
| Pessoa fazendo login no console | **User** (ou Federation/SSO) |
| Aplicação rodando em servidor on-premises | **User** com access key (ou Federation) |
| Conta AWS acessando outra conta | **Role** (cross-account) |

> **Na prova:** "EC2 sem access key" → **IAM Role**. Sempre.

---

## Estrutura de uma IAM Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "dynamodb:GetItem",
      "Resource": "arn:aws:dynamodb:us-east-1:123456789:table/MinhaTabela"
    }
  ]
}
```

| Campo | O que define |
|---|---|
| **Effect** | Allow ou Deny |
| **Action** | Quais operações (s3:GetObject, dynamodb:PutItem) |
| **Resource** | Em quais recursos (ARN específico) |
| **Condition** | Condições extras (IP, hora, MFA, etc.) |

---

## Princípio do menor privilégio

- Conceda apenas as permissões **mínimas necessárias**
- Use ARN específico em Resource (não `*`)
- Use Actions específicas (não `s3:*`)
- Na prova: "restringir acesso a apenas essa tabela" → Policy com Resource = ARN da tabela

---

## Identity-based vs Resource-based policies

| Tipo | Anexada a... | Exemplo |
|---|---|---|
| **Identity-based** | User, Group ou Role | "Este user pode ler do S3 bucket X" |
| **Resource-based** | O recurso (bucket, fila, etc.) | "Este bucket permite acesso da conta Y" |

**Quando usar Resource-based:**
- Cross-account access (conta A acessa recurso na conta B)
- S3 Bucket Policy
- SQS Queue Policy
- Lambda Resource Policy

---

## S3 Block Public Access

- Bloqueia QUALQUER configuração pública em buckets
- Pode ser aplicado por: bucket, conta, ou organização
- Impede ACLs públicas e bucket policies que exponham objetos
- "Garantir que nenhum bucket da conta seja público" → **Block Public Access no nível da conta**

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
