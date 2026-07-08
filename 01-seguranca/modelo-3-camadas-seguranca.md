# Modelo de 3 Camadas de Segurança

## O conceito

Todo requisito de segurança na prova se encaixa em uma destas 3 camadas. Identifique qual camada o cenário está pedindo antes de escolher a solução.

| Camada | Pergunta que responde | Soluções AWS |
|---|---|---|
| **Autenticação** | Quem é você? | IAM Role, IAM User, Federation, Cognito |
| **Autorização** | O que pode fazer? | IAM Policy (actions + resources com ARN) |
| **Rede** | Por onde o tráfego passa? | VPC Endpoint, Security Group, NACL, PrivateLink |

---

## Regra para a prova

Quando a questão mistura requisitos das 3 camadas, precisa de uma solução para **CADA** uma. Não tente resolver tudo com um único mecanismo.

---

## Exemplo prático

**Cenário:** EC2 precisa acessar DynamoDB sem access keys, sem internet, e só essa tabela específica.

| Requisito | Camada | Solução |
|---|---|---|
| "Sem access keys" | Autenticação | **IAM Role** associada à EC2 |
| "Só essa tabela" | Autorização | **IAM Policy** com Resource = ARN da tabela |
| "Sem internet" | Rede | **VPC Gateway Endpoint** para DynamoDB |

---

## Erro comum

Confundir IAM Role (autenticação) com VPC Endpoint (rede). São problemas diferentes:
- "EC2 sem credenciais fixas" → **IAM Role** (problema de identidade)
- "Tráfego não sai para internet" → **VPC Endpoint** (problema de rede)

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
