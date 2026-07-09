# Amazon Cognito

## O que é

Serviço de autenticação para **usuários finais de aplicações** (mobile/web). Não confundir com IAM Identity Center, que é para funcionários acessando contas AWS.

---

## Dois componentes — essa distinção cai na prova

| Componente | O que faz | Quando usar |
|---|---|---|
| **User Pool** | Diretório de usuários — cadastro, login, MFA, recuperação de senha | "Autenticar usuários da aplicação" |
| **Identity Pool** | Troca token de login por credenciais AWS temporárias | "Usuário autenticado precisa acessar S3/DynamoDB diretamente" |

---

## Fluxo típico combinando os dois

```
Usuário faz login (Google, Facebook, ou próprio)
        ↓
   Cognito User Pool  ← autentica, retorna token JWT
        ↓
  Cognito Identity Pool ← troca token por credenciais AWS temporárias
        ↓
   Usuário acessa S3 / DynamoDB / API Gateway diretamente
```

---

## Login social (Identity Federation)

Cognito suporta login via:
- Google, Facebook, Amazon, Apple (provedores sociais)
- SAML 2.0 (Active Directory corporativo, Okta, etc.)
- OpenID Connect

> Cognito faz a "ponte" entre o provedor de identidade externo e a AWS.

---

## Cognito vs IAM Identity Center

| | Cognito | IAM Identity Center |
|---|---|---|
| **Para quem** | Usuários finais de apps (clientes) | Funcionários acessando contas AWS |
| **Login social** | ✅ Google, Facebook, etc. | ❌ (usa AD corporativo ou IdP) |
| **Acesso a contas AWS** | Via Identity Pool (credenciais temporárias) | Diretamente via SSO |
| **Palavra-chave** | "app mobile/web", "login de clientes" | "login único", "SSO", "múltiplas contas" |

---

## Palavras-chave na prova

| Cenário | Serviço |
|---|---|
| "App mobile/web + autenticar usuários" | **Cognito User Pool** |
| "Usuário do app acessa S3/DynamoDB diretamente" | **Cognito Identity Pool** |
| "Login com Google/Facebook na aplicação" | **Cognito User Pool** |
| "Login único para funcionários em 50 contas AWS" | **IAM Identity Center** |
| "Active Directory corporativo + acesso a contas AWS" | **IAM Identity Center** |

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
