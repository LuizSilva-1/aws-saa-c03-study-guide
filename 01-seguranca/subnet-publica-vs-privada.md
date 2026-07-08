# Subnet Pública vs Privada

## Diferença fundamental

| Tipo | Tem acesso direto à internet? | Route Table aponta para... |
|---|---|---|
| **Pública** | ✅ Sim (entrada e saída) | Internet Gateway (IGW) |
| **Privada** | ❌ Não | NAT Gateway (só saída) ou nenhum |

A diferença é a **route table**: se tem rota para IGW → pública. Se não tem → privada.

---

## Componentes de acesso

| Componente | Direção | O que resolve |
|---|---|---|
| **Internet Gateway** | ↕️ Bidirecional | Dá acesso direto à internet para subnet pública |
| **NAT Gateway** | ➡️ Só saída | EC2 privada faz requests para internet (updates, APIs) |
| **Bastion Host** | ⬅️ Entrada SSH | Admin acessa EC2 privada via SSH |
| **ALB em subnet pública** | ⬅️ Entrada HTTP | Usuários acessam app em EC2 privada |
| **VPC Endpoint** | ➡️ Saída interna | EC2 privada acessa serviços AWS sem internet |

---

## Padrão de arquitetura comum (prova)

```
Internet
    ↓
[ALB] ← subnet pública (com IGW)
    ↓
[EC2/Fargate] ← subnet privada (sem IGW)
    ↓
[RDS] ← subnet privada (sem IGW, sem NAT)
```

- ALB na subnet pública recebe tráfego da internet
- EC2 na subnet privada processa as requisições
- RDS na subnet privada armazena dados (isolado)

---

## Regras para a prova

| Cenário | Solução |
|---|---|
| "EC2 precisa receber HTTP da internet" | ALB na subnet pública → EC2 na privada |
| "EC2 precisa baixar updates da internet" | NAT Gateway na subnet pública |
| "EC2 não pode ter acesso à internet" | Subnet privada sem NAT |
| "EC2 precisa acessar S3 sem internet" | VPC Gateway Endpoint |
| "Admin precisa SSH na EC2 privada" | Bastion Host ou Systems Manager Session Manager |

> **Regra:** Subnet privada NUNCA recebe tráfego direto da internet. Para receber HTTP → ALB na subnet pública roteando para EC2 privada.

---

## NAT Gateway vs NAT Instance

| | NAT Gateway | NAT Instance |
|---|---|---|
| **Gerenciamento** | AWS gerencia (managed) | Você gerencia (EC2) |
| **Disponibilidade** | Alta (redundante na AZ) | Precisa configurar |
| **Performance** | Até 100 Gbps | Depende do tipo de instância |
| **Na prova** | Resposta padrão | Só se pedir "menor custo" para NAT |

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
