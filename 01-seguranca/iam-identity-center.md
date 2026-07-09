# IAM Identity Center (SSO) e ACM

## IAM Identity Center

**O que é:** Serviço de Single Sign-On (SSO) para funcionários acessarem múltiplas contas AWS e aplicações SaaS com um único login.

---

## Problema que resolve

Sem IAM Identity Center, cada funcionário precisaria de um IAM User em cada conta AWS. Com 50 contas = 50 usuários por pessoa para gerenciar. Inviável.

Com IAM Identity Center:
```
Funcionário faz login UMA VEZ (com AD corporativo ou IdP)
        ↓
IAM Identity Center autentica via SAML/OIDC
        ↓
Funcionário vê portal com TODAS as contas que tem acesso
        ↓
Clica na conta → assume Role temporária → acessa
```

---

## IAM Identity Center vs Control Tower vs Organizations

| Serviço | O que faz | Palavra-chave |
|---|---|---|
| **Organizations** | Gerencia e organiza as contas (faturamento, SCPs) | "gerenciar contas", "faturamento consolidado" |
| **Control Tower** | Configura ambiente multi-conta do ZERO com boas práticas | "landing zone", "setup inicial" |
| **IAM Identity Center** | Login único (SSO) para acessar as contas | "login único", "SSO", "credenciais corporativas" |

> Control Tower usa IAM Identity Center por baixo — mas na prova são respostas diferentes para perguntas diferentes.

> Se a questão mencionar SSO ou login único diretamente → **IAM Identity Center**.
> Se mencionar landing zone ou setup multi-conta → **Control Tower**.

---

## Integração com Active Directory

IAM Identity Center integra nativamente com:
- **AWS Managed Microsoft AD** (AD na AWS)
- **AD Connector** (proxy para AD on-premises)
- Qualquer IdP SAML 2.0 (Okta, Azure AD, etc.)

**Na prova:** "credenciais corporativas do AD + acesso a múltiplas contas AWS" → **IAM Identity Center**

---

## Palavras-chave na prova

| Cenário | Serviço |
|---|---|
| "Login único para funcionários em N contas" | **IAM Identity Center** |
| "SSO para contas AWS" | **IAM Identity Center** |
| "Credenciais corporativas / AD + acesso AWS" | **IAM Identity Center** |
| "Setup inicial multi-conta com boas práticas" | **Control Tower** |
| "App mobile/web + autenticar usuários clientes" | **Cognito** |

---

## ACM — AWS Certificate Manager

**O que faz:** Cria, gerencia e renova automaticamente certificados SSL/TLS para uso em serviços AWS.

**Se associa a:**
- ALB (HTTPS no load balancer)
- CloudFront (HTTPS na distribuição)
- API Gateway (HTTPS na API)

**Vantagens:**
- Renovação automática — sem gerenciar manualmente
- Certificados públicos são **gratuitos** para uso com serviços AWS
- Certificados privados disponíveis via ACM Private CA (pago)

**Importante:** O certificado ACM **não pode ser exportado** para uso fora da AWS (ex: instalar em EC2 diretamente). Para isso, usar certificado próprio.

**Na prova:** "certificado SSL/TLS", "HTTPS no ALB/CloudFront", "renovação automática de certificado" → **ACM**

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
