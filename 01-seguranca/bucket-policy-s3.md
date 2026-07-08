# Bucket Policy no S3

## O que é

Uma **lista de regras associada ao próprio bucket** que controla quem pode acessar e sob quais condições. É como uma placa na porta de um cofre definindo quem entra e como.

---

## Os 3 cenários que caem na prova

| Cenário | O que a Bucket Policy faz |
|---|---|
| "Forçar criptografia no upload" | Deny `PutObject` sem header de criptografia SSE |
| "Forçar HTTPS / bloquear HTTP" | Deny tudo se `aws:SecureTransport = false` |
| "Acesso cross-account" | Allow para ARN de outra conta AWS |

---

## Bucket Policy vs IAM Policy

| | Bucket Policy | IAM Policy |
|---|---|---|
| **Onde fica** | No bucket (recurso) | No user/role (identidade) |
| **Controla** | "Quem pode acessar ESTE bucket" | "O que ESTE user pode acessar" |
| **Cross-account** | ✅ Sim (necessária) | ❌ Sozinha não basta |
| **Condições (HTTPS, criptografia)** | ✅ Sim | Possível mas menos comum |

---

## Quando a resposta é "Bucket Policy"

```
Quer CONDICIONAR acesso ao bucket (HTTPS, criptografia, IP)?
  → Bucket Policy

Quer dar acesso de OUTRA CONTA ao bucket?
  → Bucket Policy (resource-based)

Quer definir o que UM USUÁRIO pode fazer?
  → IAM Policy (identity-based)
```

---

## Criptografia no S3 — resumo completo

| Requisito | Solução |
|---|---|
| Criptografar objetos em repouso | **Default Encryption** (SSE-S3 ou SSE-KMS) |
| Rejeitar upload sem criptografia | **Bucket Policy** com Deny no PutObject |
| Forçar HTTPS no acesso | **Bucket Policy** com `SecureTransport` |
| Auditoria de uso de chaves | **CloudTrail** |
| Acesso ao bucket só via CloudFront | **OAC** (Origin Access Control) |

---

## Diferença: em repouso vs em trânsito

| Tipo | O que protege | Mecanismos |
|---|---|---|
| **Em repouso** | Dados ARMAZENADOS (no disco/S3) | KMS, SSE-S3, SSE-KMS, Default Encryption |
| **Em trânsito** | Dados TRAFEGANDO (na rede) | TLS, HTTPS, Bucket Policy (SecureTransport) |

São camadas independentes — o ideal é ter as DUAS.

---

## Nota sobre Default Encryption

Desde janeiro de 2023, o S3 **criptografa todos os objetos automaticamente** com SSE-S3 por padrão. Então na prova:
- "Garantir criptografia em repouso" → Default Encryption (já está ativo por padrão)
- "Garantir criptografia com chave gerenciada pelo cliente" → SSE-KMS com CMK
- "Rejeitar upload explicitamente sem criptografia" → Bucket Policy (caso mais rígido)

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
